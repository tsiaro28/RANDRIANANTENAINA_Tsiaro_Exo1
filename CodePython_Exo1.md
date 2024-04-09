from itertools import product

def table_verite(fonction, variables):
    # Générer les en-têtes de la table de vérité
    en_tetes = variables + [fonction]
    print(" | ".join(en_tetes))

    # Calculer et afficher les lignes de la table de vérité
    for ligne in product([0, 1], repeat=len(variables)):
        valeurs = list(ligne)
        valeurs.append(int(eval(fonction, dict(zip(variables, ligne)))))
        print(" | ".join(map(str, valeurs)))

def premiere_forme_canonique(variables, table_verite):
    formule = []
    for ligne in table_verite:
        if ligne[-1] == 1:
            terme = []
            for i in range(len(variables)):
                if ligne[i] == 0:
                    terme.append(variables[i])
                else:
                    terme.append(f"not {variables[i]}")
            formule.append("(" + "".join(terme) + ")")
    return " + ".join(formule)

def deuxieme_forme_canonique(variables, table_verite):
    formule = []
    for ligne in table_verite:
        if ligne[-1] == 0:
            terme = []
            for i in range(len(variables)):
                if ligne[i] == 1:
                    terme.append(variables[i])
                else:
                    terme.append(f"not {variables[i]}")
            formule.append("(" + " + ".join(terme) + ")")
    return "".join(formule)

# Nombre de variables
nb_variables = int(input("Entrez le nombre de variables : "))

# Variables de la fonction logique
print("Entrez les noms des variables séparés par des espaces :")
variables = [input(f"Variable {i+1}: ").strip() for i in range(nb_variables)]

# Fonction logique à tester 
fonction_logique = input("Entrez la fonction logique : ")

# Affichage de la table de vérité
print("\nTable de vérité :")
table_verite(fonction_logique, variables)

# Calcul et affichage des formes canoniques
table = []
for ligne in product([0, 1], repeat=len(variables)):
    valeurs = list(ligne)
    valeurs.append(int(eval(fonction_logique, dict(zip(variables, ligne)))))
    table.append(valeurs)

print("\nPremière forme canonique :")
print(premiere_forme_canonique(variables, table))

print("\nDeuxième forme canonique :")
print(deuxieme_forme_canonique(variables, table))
