# Chapitre 6 -- Le signataire : cle privee, mnemonique ou Ledger

`withdrawer` isole la signature des transactions L1 derriere une interface `Signer` a deux methodes seulement : `Address()` et `SignerFn(chainID)`. `main()` exige qu exactement une des trois options -- `--private-key`, `--mnemonic`, `--ledger` -- soit fournie ; zero ou plusieurs font echouer le programme avant meme de contacter un noeud RPC.

`CreateSigner` (dans `signer/signer.go`) construit l implementation correspondante. Une cle privee hexadecimale ou une phrase mnemonique BIP-39 aboutissent toutes deux a un `ecdsaSigner` : la mnemonique est d abord convertie en seed puis en cle privee via une derivation HD (`derivePrivateKeyFromMnemonic`, dans `signer/wallet_signer.go`), qui suit le chemin de derivation fourni par `--hd-path` (par defaut `m/44'/60'/0'/0/0`, standard Ethereum).

Le troisieme cas, un Ledger materiel, aboutit a un `walletSigner` qui delegue entierement la signature au peripherique via `usbwallet.NewLedgerHub`. Le code verifie explicitement qu un seul Ledger est connecte (erreur si zero ou plusieurs), tente de deriver le compte au chemin demande, et renvoie une erreur qui suggere de deverrouiller l appareil si la derivation echoue -- l erreur la plus probable en pratique. Dans les deux cas, `SignerFn` renvoie une fonction de signature au format attendu par `go-ethereum`, ce qui rend le reste du code (`withdraw.go`, `fpwithdraw.go`) totalement indifferent au mode de signature choisi.

Fichiers centraux : `signer/signer.go`, `signer/ecdsa_signer.go`, `signer/wallet_signer.go`.

[Chapitre suivant : configuration du gas, simulation et dry-run](07-gas-et-dry-run.md)
