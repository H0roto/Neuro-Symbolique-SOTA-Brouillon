GraphMERT: Efficient and Scalable Distillation of Reliable Knowledge Graphs from Unstructured Data Margarita Belova, Jiaxin Xiao, Shikhar Tuli and Niraj K. Jha from Department of Electrical and Computer Engineering, Princeton University ArXiv link arXiv:2510.09580 from 10 October 2025

On prend un triplet issu de UMLS : chronic kidney disease, has_finding_site, kidney structure

Ensuite, on créée manuellement un texte qui implique une connexion faible entre chronic kidney disease et cerebellar gray matter abnormalities 

Enfin, on demande à un LLM de trouver un triplet plausible (donc nous on sait que c'est (CKD, weak relation, CGM))

Le souci c'est que la majorité des LLM (comme Gemini 2.5 Pro, GPT5, Grok4, ) mettent has_finding_site en second alors que c'est une relation forte et que nous voulons une relation faible.

Solution : GraphMERT

Neural Learning : un modèle encodeur seulement qui apprend et distille à partir d'un texte spécifique au domaine, des abstractions sémantiques complexes. Celui-ci est basé sur RoBERTA

Le modèle a 12 couches cachées, 8 têtes d'attention, une taille cachée de 512 et une taille intermédiaire de couche fully connected de 2048.
Il utilise BioMedBERT comme tokenizer, a été entraîné sur un vocabulaire médical vaste, avec une taille de vocabulaire totale de 30 522 mots (sans les stop word j'imagine)

Il a été entraîné sur des abstracts de revues médicales peer-reviewed ainsi que sur des "seed KG" dérivés du UMLS Metathesaurus.

Ils ont utilisé Qwen3-32B pour chercher dans des abstracts les entités médicales qui sont intéressantes dans le cadre du dataset pris (diabete ici)
=>Ils prétendent s'affranchir des LLM pour au final l'utiliser à la première étape de leur processus ^^'
Symbolic Reasoning : Knowledge Graph
![](Pasted%20image%2020260504201047.png)

Comment faire pour qu'un transformer (RoBERTa) puisse utiliser un graphe comme si c'était une phrase?
=> Leafy chain graphs : permet d'applatir à la fois l'information syntaxique (la phrase) et l'information sémantique (triplet KG) dans une seule séquence unifiée, pouvant être utilisée comme entrée d'un encodeur.

Backbone : la chaîne de noeuds (espace syntaxique)
Branches : les "feuilles" (espace sémantique)
Connexion entre les 2 : greffer un triplet dans le graphe

![](Pasted%20image%2020260504201910.png)
![](Pasted%20image%2020260504202154.png)
![](Pasted%20image%2020260504202411.png)
![](Pasted%20image%2020260504202805.png)
Pourquoi c'est mieux qu'un décodeur ? Parce que l'encodeur est bi-directionnel (en tout cas celui-là) ce qui permet à tous les tokens de prendre en compte chaque autre token de la phrase, alors qu'un décodeur ne "regarde" que les tokens précédents.

H-GAT : Hierarchical Graph Attention : 
Pour chaque triplet, H-GAT utilise l'attention pour encoder la sémantique complète du triplet dans un "leaf embeddings", en remplaçant au passage l'ancien leaf embedding (ce qu'on a vu juste avant)
On fait tout ceci avant le processing du transformer, ce qui permet une backpropagation entre relation pendant le masked node modeling (MNM)
Ce processus se produit couche par couche, ce qui permet de capturer une vision d'ensemble de la façon dont les faits s'articulent entre eux, permettant d'avoir un KG fiable.
![](Pasted%20image%2020260505194400.png)

Ok on prend donc un texte, duquel on extrait 128 root tokens. (head)
Chaque root token peut avoir 7 feuilles reliées (tail). Ce sont des tokens directement reliés à ces root tokens, sémantiquement (par le seed graph majoritairement donc)
La relation entre les 2 est encodée par le module H-GAT (mécanisme d'attention) lorsqu'il fusionne l'embedding de la tête, de la relation et de la queue.

Après on applatit tout ça en un vecteur qu'un RoBERTa like peut lire

Efficace car à la fois MLM et MNM training (Sémantique et syntaxique)


Complexité spatiale de ce modèle : 
Espace syntaxique : espace de vecteur de la grammaire et du contexte
un vecteur de grande dimension (512)
Espace sémantique : espace de vecteur des savoirs et des faits
même dimension de vecteur mais ici contient l'embedding des noeuds feuilles

Ces 2 espaces sont connectés. se sont juste des "vues" d'un même espace de dimension très grande, 

MLM : comment passer d'un mot à l'autre grammaticalement
MNM : liens de fonctions entre 2 mots

Comment utiliser ce modèle concrètement?
1) Comme une requête SQL
2) GraphRAG : RAG mais avec un Knowledge Graph : cela contraint la réponse

![](Pasted%20image%2020260505202254.png)

![](Pasted%20image%2020260505202311.png)

Pourquoi pas 100 % de bonnes réponses pour GraphMERT ? 
A cause des LLM Helpers qui acceptent des tokens incomplets comme un "tail"  valide