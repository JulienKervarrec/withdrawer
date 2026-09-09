# Chapitre 1 -- Presentation de withdrawer

withdrawer est un utilitaire en ligne de commande, ecrit en Go, qui accomplit une seule tache : prouver puis finaliser un retrait d ETH depuis une chaine op-stack (Base ou Optimism) vers Ethereum L1. Ce n est pas une bibliotheque a integrer dans une application, c est un binaire qu on execute deux fois pour la meme transaction -- une premiere fois pour la preuve, une seconde fois pour la finalisation.

Le retrait lui-meme n est pas initie par cet outil : il commence par un envoi d ETH natif au contrat `L2StandardBridge`, a l adresse fixe `0x4200000000000000000000000000000000000010` sur L2. C est cette transaction L2 dont le hash sert d entree unique a `withdrawer`. Seul l ETH natif est supporte -- envoyer un ERC-20 a cette adresse fait perdre les fonds, avertissement repete deux fois dans le README du depot.

Entre l envoi sur L2 et la disponibilite reelle des fonds sur L1, une Challenge Period de sept jours s ecoule : le temps que le reseau ait la certitude que l etat L2 revendique est correct. C est ce delai de securite, propre a tous les rollups optimistes, que `withdrawer` aide a franchir en deux etapes espacees.

Fichiers centraux : `main.go`, `withdraw/withdraw.go`, `withdraw/fpwithdraw.go`, `withdraw/utils.go`, `signer/signer.go`.

Rien n a ete installe, compile ni execute pour ecrire ces chapitres.

[Chapitre suivant : deux chemins pour un meme retrait](02-deux-chemins.md)
