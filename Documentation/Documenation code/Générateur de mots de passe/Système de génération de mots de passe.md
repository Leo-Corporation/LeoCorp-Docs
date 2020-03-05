## Table des matières
- **[1. Introduction](#1-introduction)**
- **[2. Code](#2-code)**
- *[a. Paramètres](#a-param%C3%A8tres)*
- *[b. Génération](#b-g%C3%A9n%C3%A9ration)*
- *[c. Gérer les erreurs](#c-g%C3%A9rer-les-erreurs)*
---
## 1. Introduction
Générateur de mots de passe utilise un système de génération de mots de passe nommé "LABS_PWRG". Ce système a d'abord été développé en Visual Basic .NET, puis a été traduit en C#.
## 2. Code
### a. Paramètres
LABS_PWRG a besoin de plusieurs paramètres pour pouvoir générer un mot de passe :
| Paramètre       | Type de valeur       | Nom
| :------------- | :----------: |:---------:|
| Longueur du mot de passe | Integer   | textBoxNumber
| Caractères du mot de passe   | String | CharList
| Caractères du mot de passe (tableau)| String[] array| UsableChar[]
### b. Génération
Pour générer un mot de passe, Générateur de mots de passe utilise la fonction `GeneratePassword()`.

La longueur du mot de passe est spécifié dans le control `gunaLineTextBox1`.
Voici comment le système récupère sa valeur et la convertie en `int` :

``int textBoxNumber = int.Parse(gunaLineTextBox1.Text);``

Nous devons ensuite obtenir les caractères du mots de passe qui sont sotckés dans la variable `CharList` :

``string CharList = ("A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,a,b,c,d,e,f,g,h,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z,0,1,2,3,4,5,6,7,8,9,/,é,&,ç,à");``

Ensuite, nous devons convertir et séparer toutes ces valeurs dans un tableau. Nous allons donc utiliser la `,` en tant que séparateur.

``UsableChar = CharList.Split(new string[] { "," }, StringSplitOptions.None);``

Le tableau doit ressembler à ça :

| Index      | Valeur       |
| :--------- | :----------: |
|0|A|
|1|B|
|2|C|
|||
|...|etc|

Nous avons toutes les informations requises pour générer un mot de passe, nous pouvons désormais le générer. Pour cela, nous allons stocker notre mot de passe dans la variable `string FinalPassword = "";`. Il faut ensuite vérifié que la longueur du mot de passe (`textBoxNumber`) est plus grand que 0 :

~~~
if (textBoxNumber > 0)
{ 
    // Génère le mot de passe               
}
else
{
    // Afficher un message d'erreur
    MessageBox.Show("Le mot de passe doit être au moins long d'un caractère.", "Erreur", MessageBoxButtons.OK, MessageBoxIcon.Error);
}
~~~
Si tout est bon, on peut générer le mot de passe. Nous allons utiliser une boucle `for`, et nous allons aussi générer un nombre aléatoire nommé `Number` :
~~~
int Number = 0;
random RadomClass = new Random(); // Nombre aléatoire
Number = RandomClass.Next(0, 65); // Nombre entre 0 et 65
~~~
`Number` va nous permettre d'ajouter à la variable `FinalPassoword` un caractère présent dans ``UsableChar[]``. Par exemple, si ``Number`` = 15, le caractère se situant à l'index 15 de ``UsableChar[]`` est `P`. On ajoute alors cette lettre à `FinalPassword`, et on répète cette opération le nombre de fois que l'utilisateur a spécifié (`textBoxNumber`). Voici le code :
~~~
for (int i = 1; i < textBoxNumber; i++) // Tant que i < longueur du mot de passe
{
Number = RandomClass.Next(0, 60); // Générer un nombre aléatoire entre 0 et 60
FinalPassword = FinalPassword + UsableChar[Number]; // Ajouter au mot de passe un élément tiré au hasard de UsableChar.
}
gunaTextBox1.Text = FinalPassword; // Affiche le résultat à l'utilisateur
~~~
Voici donc comment fonctionne le système.
### c. Gérer les erreurs
Lors de la génération du mot de passe, l'utilisateur doit spécifié des valeurs. Ces valeurs peuvent ne pas êtres celle attendues par le système, comme du texte au lieu de nombre. Si c'est le cas, le système retournera une erreur, ce qui peut faire planter l'application.

Pour éviter cela, la solution est de mettre tout notre code dans un `try`. Exemple :
~~~
try 
{
    // Faire quelque chose
}
catch 
{
    // En cas d'erreur faire...
}
~~~
Cet exemple est assez simple, le programme doit faire quelque chose (dans le `try`), et si une erreur se produit, alors il doit faire quelque chose, comme avertir l'utilisateur (dans le `catch`).

On peut aussi modifier ce que le programme doit faire en fonction de l'erreur, exemple :
~~~
try 
{
    // Faire quelque chose
}
catch (FormatException ex) // Exception retournée lorsque l'utilisateur n'a pas mis un nombre de caractères.
{
    // En cas d'erreur faire...
}
catch (Exception ex) // Toutes les autres erreurs
{
    // En cas d'erreur faire...
}
~~~
Nous avons vu tous les éléments du système de génération, voici le code final :
~~~
public void GeneratePassword()
{
    UsableChar = CharList.Split(new string[] { "," }, StringSplitOptions.None);
    FinalPassword = "";
    Number = 0;
    try //Essai
    {
        int textBoxNumber = int.Parse(gunaLineTextBox1.Text); // Conversion de 'string' en 'int'
        if (textBoxNumber > 0)
        {
            for (int i = 1; i < textBoxNumber; i++) // Tant que i < longueur du mot de passe
            {
                Number = RandomClass.Next(0, 60); // Générer un nombre aléatoire entre 0 et 60
                FinalPassword = FinalPassword + UsableChar[Number]; // Ajouter au mot de passe un élément tiré au hasard de UsableChar.
            }
        gunaTextBox1.Text = FinalPassword;
        }
        else
        {
             MessageBox.Show("Le mot de passe doit être au moins long d'un caractère.", "Erreur", MessageBoxButtons.OK, MessageBoxIcon.Error);
        }
    }
    catch (FormatException ex) // En cas d'erreur où le nombre spécifié n'est pas valide
    {
        MessageBox.Show("Impossible de générer le mot de passe, car le nombre spécifié est invalide.", "Erreur", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
    catch (Exception ex) // En cas d'erreur inconnue
    {
        MessageBox.Show("Impossible de générer le mot de passe." + Environment.NewLine + ex, "Erreur", MessageBoxButtons.OK, MessageBoxIcon.Error);
    }
}
~~~