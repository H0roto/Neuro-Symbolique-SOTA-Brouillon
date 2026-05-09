https://www.youtube.com/watch?v=eHWZYURQvGw
## On commence par présenter : Supervised/Self-supervised Deep Learning (Neural Side) :
<U>Avantages : </U>

- ça permet de bien imiter la distribution d'un dataset de test
- il existe des outils assez matures
- Bonne accuracy si l'exemple ne s'éloigne pas trop des données de test
- design d'un Neural Network assez flexible

<U>Inconvénients : </U>

- On doit avoir un label pour chaque combinaison d'objets, par exemple si on veut détecter un objet dans une image
- Ces labels ne seront peut-être jamais accessibles
- La généralisation est totalement impossible : le modèle est mauvais lorsqu'il rencontre un cas qu'il ne connaît pas
- Aucune assurance que le modèle fasse exactement ce qu'on veut, même avec du Reinforcement Learning with Human Feedbacks puisque c'est compliqué de bloquer tous les chemins possibles depuis un prompt malicieux. C'est pour ça que ces modèles sont utilisés par le grand public où le coût de se tromper est plutôt faible.
- Aucune capacité à raisonner, plannifier...
- Quand un réseau de neurone est confronté à un modèle qui demande une réflexion pas à pas, plus le nombre d'étape est grand et plus le modèle est mauvais
<U>Neutre : </U>
- Fais uniquement pour la perception (selon l'auteur) car très bon pour prendre des décisions rapides avec des informations extérieures.

## Maintenant, le côté symbolique : 

<U>Principe :</U> utiliser des idées comme la logique, mathématique formelle pour raisonner.

<U>Exemples :</U> Experts Systems, Bayesian Networks, Cognitive models.

<U>Avantages : </U>
- Sont très bons pour la logique (fait pour)
- Modulaire ??? : Quand on a une théorie logique et qu'on veut ajouter un nouvel axiome à cette théorie, c'est assez facile à faire : donne moi 2 nombres X et Y tels que X + Y = 9.
Maintenant, je veux que X - Y = 2. Le fait d'ajouter une équation contraint un peu plus le problème mais ne change pas fondamentalement la façon dont on le résout.
- Ne nécessite pas de données d'entraînement spécifiques pour chaque résultat combinatoire. En gros, on est pas obligé de s'entraîner sur tous les cas possibles et inimaginables
- Là on peut facilement établir des contraintes qui seront respectées

<U>Inconvénients : </U>
- C'est très compliqué de nativement entraîner un modèle d'IA Symbolique avec des données.
- Pas tant d'outils que ça de disponibles/matures pour faire une IA Symbolique

<U>Apparte : </U>
Si l'IA Neuronale s'est autant développée, c'est aussi parce que les grosses entreprises ont vu ce que ça pourrait leur apporter, et ont contribués à développer des outils open source afin de débusquer des talents cachés, enfin augmenter le potentiel dans les gens de ce domaine.  Par exemple, Meta a contribué au développement de PyTorch.

Or, ces entreprises n'ont à ce jour pas trouvé d'intérêts réels dans les modèles logiques, et ne contribuent donc pas à leur essor.




## IA NeuroSymbolique : Combiner ces 2 mondes ensemble :

*<U>Différents buts :</U>


### 1) On peut entraîner un modèle et en même temps avoir des contraintes spécifiques pour notre domaine d'application (pendant l'entraînement donc), avec plus ou moins de souplesse suivant le domaine

Logic Tensor Networks, Logical Neural Networks, DeepProbLog, Semantic Loss, SPL, ...

On a un réseau de neurone et on veut que les poids qu'apprend le réseau adhèrent à certaines règles logiques.

On a aussi besoin de représenter le domaine de connaissance. On a 2 façons de le faire : 
- Domain knwoledge encoded as layers in the neural networks. Pour faire ça, on ajoute une couche au réseau qui représente la logique.
(Logic Tensor Networks, Logic Neural Networks)
![](Pasted%20image%2020260508145607.png)
- Domain knowledge integrated into the loss function during training :
On a une partie de la loss function qui est comme pour un réseau de neurone classique donc cross entropy ou MSE et une autre partie qui détermine à quel point on respecte les règles que l'on a établi

Contrainte plus douce avec cette approche si j'ai bien compris
![](Pasted%20image%2020260508145849.png)
### 2) On peut prendre les informations de perception et les manipuler avec de la logique
On entraîne le modèle Neuronal sur des données et on l'utilise dans un composant.
On a, comme dans notre cerveau, 2 systèmes : 
- Système 1 : reconnaître instantanément une lettre : Neural Network
- Système 2 : plus logique, mais toujours basé sur la perception : former un mot à partir des lettres qu'on reconnaît
![](Pasted%20image%2020260508150403.png)
On entraîne les modèles neuronaux tout seuls, en parallèle si on en a plusieurs comme là et c'est le module symbolique qui se sert des 2 résultats perçus pour créer un plan court, moyen ou long terme.
### 3) Utiliser la logique pour corriger la sortie d'un réseau de neurone :

On amène un ami prendre son avion, et puis sur le retour, on va dans un café. Et là, on croit reconnaître quelqu'un qui ressemble à notre ami. On se dit ensuite que ça ne peut pas être lui, puisqu'on l'a déposé dans l'avion et qu'il a décollé. Cette deuxième étape de raisonnement est ce qu'on veut instiller dans l'IA Neuronale. On ajoute donc un contexte externe à notre perception actuelle.

![](Pasted%20image%2020260508150940.png)
Là, on garde le réseau de neurone tel quel et juste on ajoute un module logique qui permet de corriger l'output précédemment émis par le réseau de neurone. On peut faire une extension de cette méthode et que l'output corrigé soit utilisé ensuite pour entraîner le réseau de neurone avec des nouveaux inputs mais bon.
### 4) Utiliser les méthodes d'optimisation neuronale (neural optimization) pour apprendre des connaissances symboliques à partir de données (ce qu'on veut faire). 
Cela s'appelle aussi du rule mining (and the APRI algorithm). C'est plus data mining que machine learning mais bon...

- Rule Learning est top pour identifier les patterns mais a des lacunes d'overfitting, de ne pas considérer les données globalement..
- Il existe des approches où on apprend des connaissances à base de symbolique comme un ensemble de règles en utilisant des descentes de gradient, et plus généralement des architectures neuronales classiques pour palier ces lacunes.


On a un réseau de neurone qui est directement capable de renvoyer en sorties des symboles logiques et le réseau lui-même peut transformer l'entrée en formules logiques.
Le processus de descente de gradient est utilisé comme un moyen d'explorer différentes règles possibles qui peuvent être apprises.

Pourquoi c'est dur en pratique ? Logique est discrète alors que les réseaux de neurone sont continues. C'est un des plus gros écarts entre les méthodes symboliques et les neuronales.
Certaines logiques permettent ce passage comme fuzzy logic, real valued logic qui mixent les valeurs scalaires et discrètes.

Pour l'instant la plupart des choses qu'on voit sont des descentes de gradients qui entraînent des simples arbres de décision. Approche top-down pour produire cette structure symbolique (arbre de décision est une structure symbolique)

Une autre approche : Differentiable Inductive Logic Programming (approche top-down aussi)

Ces 2 approches souffrent d'une complexité computationnelle assez grande, même si la première approche montre tout de même certaines promesses. Il ne faut tout de même pas oublié qu'un arbre de décision est une structrure symbolique très limitée
### 5) Analyse des raisons pour laquelle un réseau de neurone donne une certaine réponse (explicabilité)

Examiner des cas où le neurone s'active VS quand il s'active pas basé sur les informations dont on dispose.(approche mechanistique donc ça semble plus de l'interprétablilité que de l'explicabilité)

C'est une approche contre-factuelle.

Avec cette approche, on rentre dans un côté discret et donc on peut utiliser la logique


Comment savoir si on a besoin de NeuroSymbolique ?

Est-ce que mon problème bénéficie d'une connaissance additionnelle du domaine en question? (Oui)

Est-ce que cette donnée additionnelle aidera mon modèle à être un peu meilleur sur les cas qu'il n'a pas l'habitude de voir (donc généralisation)?
(On l'espère)

Est-ce que la connaissance du domaine est accessible facilement?
(Aucune idée)


Des mots à chercher : 

ABL, EDCR (Asus)