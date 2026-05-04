3 groupes de solveurs : 

**SMT Solvers (Satisfiability Modulo Theories)** : On peut utiliser le solver Z3.

Ils utilisent la logique du premier ordre comme une géante toile de contraintes algébriques et de logique booléenne. Ils sont aggressivement optimisé pour trouver si un ensemble d'équations/logiques peuvent coexister. On utilise pour cela des AST (Abstract Syntax Tree)

**Logic Programming Solvers :** On peut utiliser prolog ou pyke. Ces solvers utilisent un processus de backward/forward chaining. Cela lit une règle telle que si A alors B et cherche récursivement dans l'arbre des faits si une règle peut être divisée en sous règles

**Constraint Satisfaction Problem (CSP)**: Se concentrent sur les puzzles combinatoires. On définit un nombre fini de variables et on élimine petit à petit les possibilités

Insights : 

Le LLM doit traduire l'anglais en un language très spécifique, comme Z3Py ou PyKe => Les LLM grands publics savent comme faire ça (3 exemples suffisent)

Draft and Prune est pragmatique : traduire le brouillard mental humain en un dialecte rigide est difficile et nécessite plusieurs interprétations (20)
