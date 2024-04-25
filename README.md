# Project2_Discrete-choice-models
Discrete choice models are used to explain or predict a choice from a set of two or more discrete (i.e. distinct and separable; mutually exclusive) alternatives. 
The flu, a contagious respiratory disease with potentially severe consequences, underscores the importance of flu vaccination, as studied through factors influencing individuals' participation in vaccination programs.


Modèle logit emboîté

****1.Estimation du modèle via la commande Nlogit***

*Construire l'arbre de décision
nlogitgen vaccination = optionn(no: 1, yes: 2 | 3)
nlogittree optionn vaccination, choice(choice)

predict proba_NL // proba finale de choisir l'alternative i 
predict p* // calculer la probabilité de choix marginale
predict condp, condp hlevel(2) //calculer la probabilité de choix conditionnelle
drop proba_NL
gen proba_NL = p1*condp // p1 = marginale * condp (conditionnelle)

bys choice:sum proba_NL
predict proba_NL


*************Modèle logit mixte***************


* 1. Spécifier le modèle avec aucun effet aléatoire (aide: il faut utiliser la commande asclogit)
mixlogit choice source message incentive access, group(groupid) id(id) // il faut spécifier rand() pour que la commande fonctionne

* Construire la variable 'alternatives'. Aide: il y a une alternative par ligne pour chaque observation de choix (gid)
bys groupid: gen alternative=_n


* 2. Spécifier le modèle en incluant 3 ASC supposées aléatoires (distribution normale)
  * On crée 2 indicatrices et on met l'alternative 1(opt-out) en référence.

gen asc2 = alternative==2
gen asc3 = alternative==3

mixlogit choice source message incentive access, group(groupid) id(id) rand(asc2 asc3)

* Commenter les coefficients obtenus:
hist source
 
* 3. Spécifier le modèle avec le source comme effet aléatoire : source suit la loi normale
mixlogit choice message incentive access, group(groupid) id(id) rand(asc2 asc3 source )
