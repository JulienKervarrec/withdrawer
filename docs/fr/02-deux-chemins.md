# Chapitre 2 -- Deux chemins pour un meme retrait

`withdrawer` supporte quatre reseaux nommes -- `base-mainnet`, `base-sepolia`, `op-mainnet`, `op-sepolia` -- chacun associe a un jeu d adresses de contrats L1 fixe dans la table `networks` de `main.go`. Mais la distinction la plus importante n est pas le reseau, c est le mecanisme de verification qu il utilise : preuves optimistes classiques (`L2OutputOracle`) ou fault proofs (`DisputeGameFactory`). Le champ `faultProofs` de chaque entree du reseau tranche, et le flag `--fault-proofs` doit correspondre exactement a cette valeur -- sinon `main()` arrete tout avec `log.Crit`.

Ces deux mecanismes repondent a la meme question -- "l etat L2 revendique par ce retrait a-t-il ete valide par L1 ?" -- mais avec des garanties differentes. `L2OutputOracle` (chapitre 4) fait confiance a un proposeur unique qui publie periodiquement des racines d etat, sans mecanisme de contestation on-chain. `DisputeGameFactory` (chapitre 5) remplace cette confiance par des parties (dispute games) ou n importe qui peut contester une racine d etat proposee, avec un delai de contestation avant qu elle soit consideree finale.

Selon le mode, l appelant peut aussi fournir des adresses de contrats personnalisees (`--l2oo-address` pour le mode classique, `--dgf-address` et `--asr-address` pour les fault proofs) plutot que de s appuyer sur la table `networks` -- utile pour un reseau non repertorie.

Fichier central : `main.go`, table `networks` et validations qui suivent `flag.Parse()`.

[Chapitre suivant : le flux en deux passages, prouver puis finaliser](03-flux-principal.md)
