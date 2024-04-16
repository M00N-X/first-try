# first-try
def generer_table_de_verite(nombre_variables, fonction_logique, variables):
    table_verite = []
    for i in range(2 ** nombre_variables):
        combinaison = []
        for j in range(nombre_variables):
            combinaison.append((i >> j) & 1)
        try:
            resultat = eval(fonction_logique, dict(zip(variables, combinaison)))
        except Exception as e:
            print(f"Erreur lors de l'évaluation de la fonction logique : {e}")
            return None
        table_verite.append((combinaison, resultat))
    return table_verite

def afficher_formes_canoniques(table_verite, variables):
    if table_verite is None:
        return
    mintermes = [combinaison for combinaison, resultat in table_verite if resultat]
    fonction_premiere_forme = []
    for minterme in mintermes:
        termes = []
        for i, valeur in enumerate(minterme):
            if valeur == 1:
                termes.append(variables[i])
            elif valeur == 0:
                termes.append(f"not {variables[i]}")
        fonction_premiere_forme.append("(" + " and ".join(termes) + ")")

    maxtermes = [combinaison for combinaison, resultat in table_verite if not resultat]
    fonction_deuxieme_forme = []
    for maxterme in maxtermes:
        termes = []
        for i, valeur in enumerate(maxterme):
            if valeur == 0:
                termes.append(variables[i])
            elif valeur == 1:
                termes.append(f"not {variables[i]}")
        fonction_deuxieme_forme.append("(" + " or ".join(termes) + ")")

    print("Première forme canonique de la fonction :", " or ".join(fonction_premiere_forme))
    print("Deuxième forme canonique de la fonction :", " and ".join(fonction_deuxieme_forme))

def principale():
    while True:
        try:
            nombre_variables = int(input("Entrez le nombre de variables : "))
            if nombre_variables <= 0:
                print("Le nombre de variables doit être supérieur à zéro.")
                continue
            variables = []
            for i in range(nombre_variables):
                nom_variable = input(f"Entrez le nom de la variable {i + 1} : ")
                variables.append(nom_variable)
            fonction_logique = input("Entrez la fonction logique (en utilisant les noms des variables et les opérateurs logiques (and, not, or)) : ")
            table_verite = generer_table_de_verite(nombre_variables, fonction_logique, variables)
            if table_verite is not None:
                print("Table de vérité : ")
                for combinaison, resultat in table_verite:
                    print(combinaison, resultat)
                afficher_formes_canoniques(table_verite, variables)
            break
        except ValueError:
            print("Veuillez entrer un nombre entier pour le nombre de variables.")
        except KeyboardInterrupt:
            print("\nProgramme arrêté par l'utilisateur.")
            break
        except Exception as e:
            print(f"Une erreur s'est produite : {e}")
            break

principale()
