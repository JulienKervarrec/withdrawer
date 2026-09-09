# Parcours francais : withdrawer (preuve et finalisation des retraits L2 vers L1)

Lecture commentee du depot base/withdrawer : un utilitaire Go en ligne de commande qui prouve puis finalise un retrait d ETH depuis une chaine op-stack (Base ou Optimism) vers Ethereum L1, avec ou sans fault proofs.

Sommaire :

Chapitre 1 Presentation de withdrawer. Chapitre 2 Deux chemins pour un meme retrait. Chapitre 3 Le flux en deux passages, prouver puis finaliser. Chapitre 4 Le retrait classique, L2OutputOracle et OptimismPortal. Chapitre 5 Le retrait avec fault proofs, DisputeGameFactory et parties invalidees. Chapitre 6 Le signataire, cle privee, mnemonique ou Ledger. Chapitre 7 Configuration du gas, simulation et dry-run. Chapitre 8 Limites et perimetre de ce parcours.

Ce parcours est une lecture pedagogique du code source et de la documentation du depot, sans installation ni execution du projet.
