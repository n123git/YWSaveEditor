**Note**: this should be quite obvious, but this is only for decryption, consult [README.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/README.md) for more.

# Game Files (game*.yw)

## v0.0 Save Files
The YW2 Demo dosen't save progress. This is a joke, ignore this (I refuse to remove it).

## v1.0 Save Files
In the international versions, this format affects save files last saved in v1.0. They can be read by v2.0 game copies. In the JP versions, it describes any version under 2.0, as the version history is different. Note that all copies of _Psychic Specters_ or _Shin'uchi_ are v2.0. A save file will have v2.0 marked on it in-game if it isn't.
* These are first decrypted via a slighly non-standard AES-CCM (not GCM or plain CTR). The aeskey is fixed in v1.0 saves: "5+NI8WVq09V7LI5w". Then it uses a proprietary cipher, which I refer to as "YWCipher" inspired by Togenyan's naming schema. Then the CRC and key are stripped.


## v2.0 Save Files
This format affects save files last saved in v2.0 (or higher due to JP version history). Note that all copies of _Psychic Specters_ or _Shin'uchi_ (regardless of update) are v2.0. A save file will have v2.0 marked on it in-game if it is. The main difference is that the AESkey is no longer fixed, it is instead loaded from the `head.yw`. Specifically it gets the headdata, and places it into a fucntion that treats it as {ciphertext, CRC value of ciphertext, encryption key}. First, it extracts the last 8 bytes:
* 4 bytes CRC32 of the ciphertext
* 4 bytes encryption key
Then it removes/strips them as they are no longer important. Verifies the integrity of the ciphertext by checking that its calculated CRC matches the given CRC. If it doesn't match → it will return NULL, meaning the decryption fails. It then uses the proprietary `YWCipher` to decrypt it using the extracted key and then appends the original CRC + key (the last 8 bytes of the input) back into to the decrypted data. Now, it reads a uint32 (32-bit unsigned integer) from the decrypted file starting at offset `0x0C` (12). Then uses it as a seed for XORshift PRNG to generate 16 bytes continuosly until it gets a 128-bit AES key.



Here is a slightly readjusted snippet from Togenyan's save editor, the appropriate license is placed next to this `.md`. This snippet detects which version the save file is, and adjusts it accordingly.
```cpp
if (encrypted) {  // Is it an encrypted save
        this->mgr->setAeskey("5+NI8WVq09V7LI5w"); // test with the hardcoded key used in v1.0 saves
        if ((status = this->mgr->loadFile(file)) != Error::SUCCESS) {
            // If that fails, assume Ganso / Honke ver 2.x OR Shin'uchi
            if ((status = this->mgr->loadKeyFromHeadFile(file)) == Error::SUCCESS) {
                status = this->mgr->loadFile(file);
            }
        }
    } else { // If decrypted
        status = this->mgr->loadDecryptedFile(file); // just edit it
    }
    if (status != Error::SUCCESS) { // if it fails
        QMessageBox::critical(this, tr("ERROR"), QString(tr("ERROR (%1)")).arg(status)); // have a tantrum
        this->mgr->setAeskey(prevKey); // restore the key
        return; // exit
    }
```

Here is an example depicting the general decryption process (again from Togenyan's save editor, but slightly readjusted. MIT license is goated). 

```cpp
Error::ErrorCode SaveManager::loadFile(QString path)
{
    QFile file(path);

    if (!file.open(QIODevice::ReadOnly)) { // Error handling, ignore this
        return Error::FILE_CANNOT_OPEN;
    }

    QByteArray bodydata = file.readAll(); // get the file.... not really complicated
    file.close();

    // key
    QByteArray nonce;

    // decrypt first layer (AES CCM)
    nonce = bodydata.left(0x0C); // get the nonce
    CCMCipher myCCM(this->aeskey, nonce); // the aeskey depends, read the previous example for more info.
    QByteArray *decryptedFirst = myCCM.decrypt(bodydata.right(bodydata.size() - 0x10));
    if (!decryptedFirst) {
        return Error::DECRYPTION_CCM_FAILED;
    }

    // decrypt second layer (YWCipher)
    QByteArray ywkeyBytes = decryptedFirst->right(4);
    QByteArray *decryptedSecond = SaveManager::processYW(*decryptedFirst, false);
    delete decryptedFirst;
    if (!decryptedSecond) {
        return Error::DECRYPTION_YW_FAILED;
    }

    // strip CRC + key
    decryptedSecond->resize(decryptedSecond->size() - 8);

    // split into sections
    Error::ErrorCode status = this->parseSavedata(*decryptedSecond);

    delete decryptedSecond;
    if (status != Error::SUCCESS) {
        return status;
    }

    // loaded successfully
    this->filepath = path;
    this->nonce = nonce;
    this->ywcipherKey = ywkeyBytes;
    this->isLoaded = true;

    return Error::SUCCESS;
}
```

# Header Files (head.yw)
These are decrypted in the same way as yw1 saves, specifically they are decrypted identically to v1.0 saves but without the AES encryption at ALL, just `YWCipher`. Here is an example from Togenyan's YW1 Save Editor:
