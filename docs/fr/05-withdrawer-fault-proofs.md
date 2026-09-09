# Chapitre 5 -- Le retrait avec fault proofs : DisputeGameFactory et parties invalidees

`FPWithdrawer` (dans `withdraw/fpwithdraw.go`) remplace `L2OutputOracle` par `DisputeGameFactory` et `OptimismPortal2`, et ajoute une dependance a `AnchorStateRegistry`. `CheckIfProvable` appelle `withdrawals.FindLatestGame` pour recuperer la derniere partie de contestation active, puis extrait le numero de bloc L2 revendique directement des 32 premiers octets de son `ExtraData` -- une lecture bas niveau plutot qu un champ nomme, parce que le format d `ExtraData` est une convention du DisputeGame, pas un getter expose.

Difference notable avec le mode classique : `ProvenWithdrawals` est desormais interroge avec deux cles -- le hash du retrait ET l adresse de l appelant (`w.Opts.From`) -- au lieu du seul hash. Consequence directe expliquee au chapitre 2 de ce depot dans son historique : plusieurs adresses differentes peuvent proposer une preuve pour le meme retrait, chacune associee a sa propre partie de contestation.

C est ce qui rend necessaire `isDisputeGameInvalidated` : avant de considerer une preuve existante comme valable, `GetProvenWithdrawalTime` verifie aupres de l `AnchorStateRegistry` que la partie de contestation associee n est ni blacklistee (`IsGameBlacklisted`) ni retiree (`IsGameRetired`). Si elle l est, la fonction retourne zero comme si aucune preuve n existait -- ce qui, dans le flux du chapitre 3, redeclenche naturellement une nouvelle preuve plutot que de tenter une finalisation vouee a l echec. `FinalizeWithdrawal` ajoute egalement un appel explicite a `CheckWithdrawal` avant de construire la transaction de finalisation, verification absente du chemin classique.

Fichier central : `withdraw/fpwithdraw.go`.

[Chapitre suivant : le signataire, trois facons de signer une transaction](06-signer.md)
