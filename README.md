# Boom
Jeu
import pygame
import random

# Initialisation de Pygame
pygame.init()

# Définir les dimensions de la fenêtre
largeur_fenetre = 800
hauteur_fenetre = 600

# Couleurs
BLANC = (255, 255, 255)
NOIR = (0, 0, 0)
ROUGE = (213, 50, 80)
VERT = (0, 255, 0)
BLEU = (50, 153, 213)

# Définir la taille du serpent et la vitesse
taille_case = 10
vitesse_serpent = 15

# Définir la police de texte
police_score = pygame.font.SysFont("bahnschrift", 25)

# Créer la fenêtre de jeu
fenetre = pygame.display.set_mode((largeur_fenetre, hauteur_fenetre))
pygame.display.set_caption('Jeu Snake')

# Fonction pour afficher le score
def afficher_score(score):
    valeur = police_score.render("Score : " + str(score), True, NOIR)
    fenetre.blit(valeur, [0, 0])

# Fonction pour dessiner le serpent
def dessiner_serpent(taille_case, serpent):
    for x in serpent:
        pygame.draw.rect(fenetre, VERT, [x[0], x[1], taille_case, taille_case])

# Fonction principale du jeu
def jeu():
    game_over = False
    game_close = False

    # Position initiale du serpent
    x1 = largeur_fenetre / 2
    y1 = hauteur_fenetre / 2

    x1_change = 0
    y1_change = 0

    serpent = []
    longueur_serpent = 1

    # Position initiale de la nourriture
    nourriturex = round(random.randrange(0, largeur_fenetre - taille_case) / 10.0) * 10.0
    nourriturey = round(random.randrange(0, hauteur_fenetre - taille_case) / 10.0) * 10.0

    clock = pygame.time.Clock()

    while not game_over:

        while game_close == True:
            fenetre.fill(BLEU)
            message = police_score.render("Vous avez perdu! Appuyez sur Q-Quitter ou C-Rejouer", True, ROUGE)
            fenetre.blit(message, [largeur_fenetre / 6, hauteur_fenetre / 3])
            afficher_score(longueur_serpent - 1)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        jeu()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x1_change = -taille_case
                    y1_change = 0
                elif event.key == pygame.K_RIGHT:
                    x1_change = taille_case
                    y1_change = 0
                elif event.key == pygame.K_UP:
                    y1_change = -taille_case
                    x1_change = 0
                elif event.key == pygame.K_DOWN:
                    y1_change = taille_case
                    x1_change = 0

        if x1 >= largeur_fenetre or x1 < 0 or y1 >= hauteur_fenetre or y1 < 0:
            game_close = True
        x1 += x1_change
        y1 += y1_change
        fenetre.fill(BLEU)
        pygame.draw.rect(fenetre, ROUGE, [nourriturex, nourriturey, taille_case, taille_case])

        serpent_tete = []
        serpent_tete.append(x1)
        serpent_tete.append(y1)
        serpent.append(serpent_tete)
        if len(serpent) > longueur_serpent:
            del serpent[0]

        for x in serpent[:-1]:
            if x == serpent_tete:
                game_close = True

        dessiner_serpent(taille_case, serpent)
        afficher_score(longueur_serpent - 1)

        pygame.display.update()

        # Si le serpent mange la nourriture
        if x1 == nourriturex and y1 == nourriturey:
            nourriturex = round(random.randrange(0, largeur_fenetre - taille_case) / 10.0) * 10.0
            nourriturey = round(random.randrange(0, hauteur_fenetre - taille_case) / 10.0) * 10.0
            longueur_serpent += 1

        clock.tick(vitesse_serpent)

    pygame.quit()
    quit()

# Lancer le jeu
jeu()
