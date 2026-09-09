# Chapitre 4 -- Le retrait classique : L2OutputOracle et OptimismPortal

`Withdrawer` (dans `withdraw/withdraw.go`) implemente le chemin sans fault proofs. `CheckIfProvable` interroge `L2OutputOracle` pour trois valeurs : l intervalle de soumission, la duree d un bloc L2, et le numero du dernier bloc L2 propose. Si ce dernier bloc propose est encore anterieur au bloc qui contient le retrait, la fonction retourne une erreur qui indique precisement combien de temps attendre -- calcule a partir de l intervalle de soumission et de la duree de bloc, pas une simple estimation.

`getWithdrawalHash` retrouve le hash unique du retrait a partir du recu de transaction L2 : il localise l evenement `MessagePassed` emis par le contrat L2ToL1MessagePasser, puis derive le hash via `withdrawals.WithdrawalHash`. Ce hash sert de cle pour interroger `ProvenWithdrawals` sur le `OptimismPortal` -- c est ainsi que `GetProvenWithdrawalTime` sait si une preuve existe deja.

`ProveWithdrawal` construit les parametres de preuve Merkle (`ProveWithdrawalParameters`, fournis par la bibliotheque `op-node`) a partir du dernier bloc L2 propose, puis appelle `ProveWithdrawalTransaction` sur le portail. `FinalizeWithdrawal` refait un calcul similaire mais ajoute une verification explicite du delai : si `l2WithdrawalBlock.Time + finalizationPeriod >= l1Head.Time`, la fonction refuse et explique l ecart de temps restant plutot que de laisser la transaction L1 echouer inutilement en consommant du gas.

Fichier central : `withdraw/withdraw.go`.

[Chapitre suivant : le retrait avec fault proofs et les parties de contestation](05-withdrawer-fault-proofs.md)
