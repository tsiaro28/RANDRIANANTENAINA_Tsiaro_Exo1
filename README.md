from itertools import product

# Définir la fonction logique
def fonction_logique(A, B, C):
    return (A and B ) or (not C)

# Générer la table de vérité avec des en-têtes
def table_de_verite(func):
    variables = ['A', 'B', 'C', 'f']
    print('\t'.join(variables))
    for valeurs in product([True, False], repeat=len(variables)-1):
        resultat = func(*valeurs)
        valeurs_str = '\t'.join([str(int(val)) for val in valeurs] + [str(int(resultat))])
        print(valeurs_str)

# Trouver les formes canoniques
def formes_canoniques(func):
    dnf_terms = []
    cnf_terms = []
    for valeurs in product([True, False], repeat=3):
        term_dnf = "(" + "".join([f"{var}" if val else f"~{var}" for i, (val, var) in enumerate(zip(valeurs, ['A', 'B', 'C']))]) + ")"
        term_cnf = "(" + " +".join([f"~{var}" if val else f"{var}" for i, (val, var) in enumerate(zip(valeurs, ['A', 'B', 'C']))]) + ")"
        if func(*valeurs):
            dnf_terms.append(term_dnf)
        else:
            cnf_terms.append(term_cnf)
    
    dnf_expression = " + ".join(dnf_terms)
    cnf_expression = "".join(cnf_terms)
    
    return dnf_expression, cnf_expression

# Afficher la table de vérité
print("Table de vérité:")
table_de_verite(fonction_logique)

# Afficher les formes canoniques avec les vraies expressions
print("\nPremière forme canonique (FCD) :")
dnf_expression, _ = formes_canoniques(fonction_logique)
print(dnf_expression)

print("\nDeuxième forme canonique (FCC) :")
_, cnf_expression = formes_canoniques(fonction_logique)
print(cnf_expression)
