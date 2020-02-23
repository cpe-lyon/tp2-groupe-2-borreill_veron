#  Exercice 1. Variables d’environnement

1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?
> echo $PATH
>/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ?
> par défaut, cela renvoie à $HOME qui est le répertoire personnel
3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.
> LANG correspond à la langue et le codage des caractères
> PWD renvoie le répertoire courant
> OLDPWD renvoie le répertoire courant précédent
> SHELL le chemin de bash (/bin/bash)
> _ renvoie la dernière variable d'environnement 
4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe
> MY_VAR='toto'
5. Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale. 
> on rentre dans un nouveau bash. La variable MY_VAR n'existe pas car elle est local au shell sur lequel elle a été créée (variable de shell)
6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez. 
> export MY_VAR si elle est déjà crée
> sinon export MY_VAR='toto'
7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte. 
> export NOMS="louis yohann"
> echo $NOMS
> l'affectation est correcte
8. Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS. 
> echo '"bonjour à vous deux, '$NOMS'!"' 
9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset ? 
>  unset NOMS ( pas de $) supprime la variable
>  Si on met une valeur vide à la variable, La variable n'a pas de valeur mais existe toujours
10. Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash)
> echo '$HOME = '$HOME

# Programmation Bash
Ajoutez le chemin vers script à votre PATH de manière permanente
> dans le fichier ~/.bashrc, ajout de la ligne : 
> PATH=$PATH":/root/script"

# Exercice 2. Contrôle de mot de passe
Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.
>```bash
>#!/bin/bash
>
>PASSWORD="123"
>stty -echo
>read -p "Entrer le mot de passe: " PASS
>echo ""
>if [ $PASS == $PASSWORD ]
>then
>echo "le password est bon"
>else
>echo "t'es mauvais JACK"
>fi
>stty echo
>```

# Exercice 3. Expressions rationnelles
 Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel : function is_number() 
 ```bash
 { 
	 re='^[+-]?[0-9]+([.][0-9]+)?$' 
if ! [[ $1 =~ $re ]] ; then
	 return 1 
 else 
	 return 0
  fi 
	  } 
```
  Il affichera un message d’erreur dans le cas contraire.

#!/bin/bash

function is_number
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ;
	then
		return 1
	else
		return 0
	fi
}
is_number $1
if is_number $1
#if ["$?" -eq "0"]
then
	echo $1" is a number"
else
	echo $1" is not a number"
fi
```

# Exercice 4. Contrôle d'utilisateur
Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)
```bash
#!/bin/bash
nom=$1
script=$0

if [ -z "$nom" ]
then
	echo "Utilisation : "$script "nom_utilisateur"
else
	id -u $nom
	if [ "$?" -eq 1 ]
	then
		echo "L'utilisateur n'existe pas"
	else	
		echo "Utilisation : "$script $nom
	fi
fi
```

# Exercice 5. Factorielle
Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).
```bash
#!/bin/bash
n=$1
facto=1
if [ "$n" -lt 2 ]
then
	echo "votre nombre est trop petit"
else
	while [ "$n" -gt 0 ]
	do
		facto=$(($facto*$n))
		n=$(($n-1))
	done
	echo "factoriel de "$1" = "$facto
fi
```

# Exercice 6. Le juste prix
Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner. Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).
```bash
#!/bin/bash
prix=$RANDOM
max=1000
while [ "$prix" -gt "$max" ]
do
	prix=$RANDOM
	#echo $prix
done
read -p "Entrez une valeur entre 0 et 1000€: " VALUE
while [ "$VALUE" -ne "$prix" ]
do
	if [ "$VALUE" -gt "$prix" ]
	then
		echo "C'EST MOINS! "
		read -p "Entrez une valeur entre 0 et 1000€: " VALUE
	elif [ "$VALUE" -lt "$prix" ]
	then
		echo "C'EST PLUS! "
		read -p "Entrez une valeur entre 0 et 1000€: " VALUE
	else
		echo "c kc"
	fi
done
echo "T'es le boss"
```
# Exercice 7. Statistiques
1. Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers. 
```bash
#!/bin/bash

function is_number
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ;
then
	return 1
else
	return 0
fi
}
if is_number $1 && is_number $2 && is_number $3
then
	if [ "$1" -lt "$2" ] && [ "$1" -lt "$3" ]
	then
		echo "Le min est "$1
	elif [ "$2" -lt "$1" ] && [ "$2" -lt "$3" ]
	then
		echo "Le min est "$2
	elif [ "$3" -lt "$1" ] && [ "$3" -lt "$2" ]
	then
		echo "Le min est "$3
	fi
	if [ "$1" -gt "$2" ] && [ "$1" -gt "$3" ]
	then
		echo "Le max est "$1
	elif [ "$2" -gt "$1" ] && [ "$2" -gt "$3" ]
	then
		echo "Le max est "$2
	elif [ "$3" -gt "$1" ] && [ "$3" -gt "$2" ]
	then
		echo "Le max est "$3
	fi
	moyenne=$((($1+$2+$3)/3))
	echo "La moyenne est: "$moyenne
	else
		echo "Veuillez rentrer 3 entiers entre -100 et 100"
fi
```
2. Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT) 
3. Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et à mesure dans un tableau.
```bash
#!/bin/bash

function is_number
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ;
then
	return 1
else
	return 0
fi
}

minimum=100
maximum=-100
moyenne=0
somme_moyenne=0


nombre=0

for i in $(seq 1 $1);do

read -p "Choisissez une valeur:" VALUE

    if [[ $VALUE -gt 100 ] && [ $VALUE -lt "-100" ] && [ is_number $VALUE ]] ; then
        echo "Veuillez rentrer un entier entre -100 et 100"
        
       
    else
  
    if [ $VALUE -lt $minimum ]; then
   
    minimum=$VALUE;   

    elif [ $VALUE -gt $maximum ]   
    maximum=$VALUE;

    fi


    somme_moyenne=$(($somme_moyenne+$VALUE))
	nombre=$(($nombre+1))
   
    shift
   
    fi
   



done
    moy=$(($somme_moyenne/$nombre))
    echo "Le max est "$moy
    echo "Le min est "$maximum
    echo "La moyenne est: "$moyenne
```