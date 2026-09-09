# Chapitre 3 -- Le flux en deux passages : prouver puis finaliser

Le corps de `main()` deroule toujours la meme sequence de quatre questions, quel que soit le mode. D abord, `IsProofFinalized()` : si le retrait est deja finalise, le programme s arrete immediatement en l indiquant -- executer `withdrawer` une troisieme fois sur un retrait deja complete ne fait rien de destructeur.

Ensuite, `CheckIfProvable()` verifie que l etat L2 necessaire a la preuve a bien ete publie sur L1 (une racine d etat ou une partie de contestation qui couvre le bloc du retrait) ; sinon le programme s arrete avec un message expliquant combien de temps attendre encore.

Puis `GetProvenWithdrawalTime()` regarde si une preuve existe deja pour ce retrait. Si le temps retourne est zero, `ProveWithdrawal()` est appele : c est le premier des deux passages, celui qui peut avoir lieu des que l etat L2 est disponible sur L1. Si un temps non nul existe deja, le programme saute directement a `FinalizeWithdrawal()` : c est le second passage, qui ne peut reussir qu apres l ecoulement de la Challenge Period (et, en mode fault proofs, apres resolution de la partie de contestation en faveur de la revendication).

Cette structure en deux passages explique pourquoi `withdrawer` s execute typiquement deux fois avec exactement la meme commande, a plusieurs jours d intervalle : le programme decide lui-meme, a partir de l etat on-chain, s il doit prouver ou finaliser.

Fichier central : `main.go`, fonction `main()` a partir de l appel a `withdrawer.IsProofFinalized()`.

[Chapitre suivant : le retrait classique, L2OutputOracle](04-withdrawer-classique.md)
