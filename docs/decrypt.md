**Note**: this should be quite obvious, but this is only for decryption, consult [README.md](https://github.com/n123git/YWSaveEditor/blob/main/docs/README.md) for more.

# Game Files (game*.yw)

## v1.0 Save Files
In the international versions, this format affects save files last saved in v1.0. They can be read by v2.0 game copies. In the JP versions, it describes any version under 2.0, as the version history is different. Note that all copies of _Psychic Specters_ or _Shin'uchi_ are v2.0. A save file will have v2.0 marked on it in-game if it isn't.
* These are first decrypted via AES-CCM (not GCM or plain CTR). The key is derived from blabla

## v2.0 Save Files
This format affects save files last saved in v2.0 (or higher due to JP version history). Note that all copies of _Psychic Specters_ or _Shin'uchi_ (regardless of update) are v2.0. A save file will have v2.0 marked on it in-game if it is. The main difference is 


Here is a slightly readjusted snippet from Togenyan's save editor, the appropriate license is placed next to this `.md`
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

# Header Files (head.yw)
These are decrypted in the same way as yw1 saves, specifically.....
