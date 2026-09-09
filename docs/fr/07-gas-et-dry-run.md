# Chapitre 7 -- Configuration du gas, simulation et dry-run

Au-dela des flags de base, `withdrawer` expose un jeu complet d options de gas : `--gas-limit` pour figer une limite, `--gas-price` pour une transaction legacy, `--max-fee-per-gas`/`--max-priority-fee` pour l EIP-1559, `--gas-multiplier` pour ajouter une marge a une estimation automatique, et `--max-gas-price` comme plafond de securite. `main()` valide ces combinaisons avant tout appel reseau : `--gas-price` est incompatible avec les flags EIP-1559, les deux flags EIP-1559 doivent etre fournis ensemble, et le multiplicateur doit rester superieur ou egal a 1.0.

La fonction cle est `prepareGasOpts` (`withdraw/utils.go`), appelee juste avant chaque transaction (`ProveWithdrawal` et `FinalizeWithdrawal`, dans les deux implementations). Elle reinitialise d abord la limite de gas a la valeur utilisateur (ou zero pour laisser l estimation automatique jouer), puis simule la transaction avec `NoSend: true` des que le dry-run est demande ou qu un multiplicateur superieur a 1.0 doit s appliquer a une estimation automatique. Cette simulation retourne une transaction jamais envoyee, dont le gas estime sert a calculer la limite ajustee.

En mode `--dry-run`, `printDryRun` affiche le detail de la transaction simulee -- destinataire, valeur, gas estime, cout maximal en ETH calcule a partir du type de transaction (legacy ou EIP-1559) -- et le programme s arrete la, sans jamais appeler `w.Opts` sur une transaction reelle. C est le seul endroit du programme qui touche a de l argent sans le depenser : chaque autre chemin du code aboutit, tot ou tard, a l envoi effectif d une transaction signee.

Fichier central : `withdraw/utils.go`, fonctions `prepareGasOpts` et `printDryRun`.

[Chapitre suivant : limites et perimetre de ce parcours](08-limites-perimetre.md)
