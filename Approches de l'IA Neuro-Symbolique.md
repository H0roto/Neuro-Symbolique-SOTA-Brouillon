https://www.youtube.com/watch?v=AWjpaYZZMZc
1) Utiliser l'IA symbolique pour guider l'entraînement de l'IA Neuronale
2) Utiliser l'IA Symbolique pour contrôler les résultats de l'IA Neuronale pendant son utilisation

Un agent LLM interroge un Module Symbolique externe pour une réponse logique

Réseaux Neuronaux Logiques (LNN) : chaque neurone a une signification précise. Il correspond à un élément d'une formule logique


Les 3 promesses de l'IA NS :
* Résoudre des problèmes plus difficiles
* Apprendre avec moins de données
* Fournir des décisions compréhensibles ert contrôlables

Limites : 
* Grande complexité de calcul
* Difficulté d'intégration transparente
* Manque de bancs d'essai standardisés
* Equilibre performance/transparence

Combine la structure logique avec le pouvoir d'apprentissage adaptatif



2 piliers fondamentaux

Symbolique : représentation explicite des connaissances
Bons pour : raisonnements logiques, explicabilité, résoudre des problèmes structurés comme les mathématiques
Mauvais pour : données non structurées comme les images ou la parole qui demandent de la programmation manuelle
RIGIDE


Neuronale : 
Bons pour : vision, langage, reconnaissance du langage, reconnaissance de patterns, apprentissage par l'exemple et s'améliorent avec un grand nombre de données
Mauvais pour : explicabilité, raisonnement logique, généralisation en dehors des données d'exemple.

flexible pattern recognition with interpretable reasoning

![](Pasted%20image%2020260502164259.png)

Perception : transforme les données brutes non structurées en données symboliques structurées
Reasoning : Applique une inférence de logique et des règles structurées à ces données structurées précédemment extraites
Learning and Feedback layer : combine un raisonnement symbolique et un feedback neuronal ???? x)))


L'IA Neuronale ne raisonne pas. Elle connaît juste les relations entre plusieurs données

![](Pasted%20image%2020260502170419.png)

Les systèmes neuro-symbolique peuvent raisonner
Mets à jour ses connaissances sans nécessiter des millions d'exemple : Meta-learning 

Neural Layer => Logic Layer => Decision

On peut introduire ça dans les modèles pré-entraînés ou dans du reinforcement learning aussi
Cela rend les systèmes plus explicables et robustes

MIT-IBM Neuro-Symbolic Concept Learner


Logic-LM : Empowering LLM with Symbolic Sovers ofr Faithful logical reasoning
=> Ne performe pas bien du tout...

Draft and Prune : Improving the Reliability of Auto-formalization for logical reasoning

AutoFormalization : traduit le processus humain de raisonnement naturel dans un raisonneur numérique, symbolique et exécutable => Permet au solver symbolique d'avoir une vraie profonde déduction logique

Problème actuel : Pipeline IA fragile : échoue à s'exécuter ou s'exécute avec le mauvais raisonnement humain

Ce n'est pas un problème de l'architecture transformer puisque celle-ci n'a tout simplement pas été designé pour cela. Il faut donc changer de fonctionnement interne :

On reste ici sur le système : d'abord Transformer puis vérificateur symbolique pour diriger la réponse

"Data Starvation Wall" of Neuro-Symbolic AI

Les auteurs transfèrent la charge de travail du temps d'entraînement vers l'inférence :
*Du coup c'est le client qui acquiert cette charge ou pas?*

1) 
On utilise donc le LLM uniquement comme un apriori stochastique

3-Shot Learning utilisé

On doit connecter le LLM à un solver externe (prolog, lean4, etc.)

Cette méthode n'a pas fonctionné apparemment

2)  On utilise l'hallucination contrôlée d'un LLM : on compile 20 plans (différentes interprétations sémantiques) et on les transforme en 20 programmes formels. La plupart seront faux mais ça nous va (18 ne marcheront pas pour avoir une proportion)
En gros, on teste pas mal de possibilités et certaines marcheront : le solver les considèrera pas comme contradiction ou ambigüe

Le plus dur là-dedans c'est de combler le vide entre les pensées humaines bordéliques et le raisonnement froid mathématique
Cette technique (Draft And Prune) évite le processus fastidieux de Fine-Tuning supervisé

On est dans un In-Context Learning

En gros le LLM teste un peu toutes les possibilités et si le solver dit que c'est bon bah c'est gagné x)

Les LLM approximent le raisonnement ne mappant des patterns linguistiques dans d'autres patterns linguistiques

A partir du moment où un problème est transformé en FOL, on enlève les hallucinations, les oublis et les biais (d'après l'auteur de la vidéo en tout cas)


![](Pasted%20image%2020260503101625.png)
![](Pasted%20image%2020260503101927.png)

ça permet de se rendre compte qu'un LLM sait écrire de la logique mais sans pruning il écrit une logique souvent incohérente
![](Pasted%20image%2020260503102025.png)


On peut aussi utiliser un Knowledge Graph : L'IA fait une requête en langage naturel. Elle appelle ensuite un Knowledge graph qui permet de retourner une donnée précise, vérifiée. 
L'IA l'utilise ensuite pour générer une réponse correcte. Le truc c'est que pour que le LLM soit en accord avec les données du KG, il faudrait qu'il soit continuellement entraîné sur ce KG...

Il faut des KG très gros