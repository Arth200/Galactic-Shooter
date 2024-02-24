import pygame
import random
import time

# Initialisation de Pygame
pygame.init()

# Définition des couleurs
BLANC = (255, 255, 255)
NOIR = (0, 0, 0)
ROUGE = (255, 0, 0)
VERT = (0, 255, 0)
GRIS = (128, 128, 128)
BLEU = (0, 0, 255)

# Définition des paramètres de la fenêtre
largeur_fenetre = 800
hauteur_fenetre = 600

# Classe du vaisseau
class Vaisseau(pygame.sprite.Sprite):
    def __init__(self, taille):
        super().__init__()
        self.taille = taille
        self.image = pygame.Surface([self.taille[0], self.taille[1]])
        self.image.fill(BLANC)
        self.rect = self.image.get_rect()
        self.rect.x = 10
        self.rect.y = 10

    def update(self):
        pos = pygame.mouse.get_pos()
        self.rect.y = pos[1]

# Classe du projectile
class Projectile(pygame.sprite.Sprite):
    def __init__(self, x, y, taille):
        super().__init__()
        self.taille = taille
        self.image = pygame.Surface([self.taille[0], self.taille[1]])
        self.image.fill(ROUGE)
        self.rect = self.image.get_rect()
        self.rect.x = x
        self.rect.y = y

    def update(self):
        self.rect.x += 10
        if self.rect.x > largeur_fenetre:
            self.kill()

# Classe de l'ennemi
class Ennemi(pygame.sprite.Sprite):
    def __init__(self, taille, vitesse):
        super().__init__()
        self.taille = taille
        self.image = pygame.Surface([self.taille[0], self.taille[1]])
        self.image.fill(BLEU)
        self.rect = self.image.get_rect()
        self.rect.x = largeur_fenetre
        self.rect.y = random.randrange(hauteur_fenetre - self.rect.height)
        self.vitesse = vitesse

    def update(self):
        self.rect.x -= self.vitesse

# Fonction de génération d'un nouvel ennemi
def spawn_enemy(ennemis, tous_les_sprites, taille, vitesse):
    ennemi = Ennemi(taille, vitesse)
    ennemis.add(ennemi)
    tous_les_sprites.add(ennemi)

# Fonction pour afficher le menu
def afficher_menu(fenetre, font, option_index):
    fenetre.fill(NOIR)
    texte_titre = font.render("Menu", True, BLANC)
    texte_titre_rect = texte_titre.get_rect(center=(largeur_fenetre // 2, hauteur_fenetre // 4))
    fenetre.blit(texte_titre, texte_titre_rect)

    y_position = hauteur_fenetre // 2
    for index, niveau in enumerate(difficulty_settings.keys()):
        texte_niveau = font.render(niveau, True, VERT if index == option_index else BLANC)
        texte_niveau_rect = texte_niveau.get_rect(center=(largeur_fenetre // 2, y_position))
        fenetre.blit(texte_niveau, texte_niveau_rect)
        y_position += 50

    pygame.display.flip()

# Fonction pour afficher l'écran de défaite
def afficher_defaite(fenetre, font, score):
    fenetre.fill(NOIR)
    texte_defaite = font.render("Vous avez perdu!", True, ROUGE)
    texte_defaite_rect = texte_defaite.get_rect(center=(largeur_fenetre // 2, hauteur_fenetre // 2))
    fenetre.blit(texte_defaite, texte_defaite_rect)

    # Afficher le score
    texte_score = font.render("Score: {}".format(score), True, BLANC)
    texte_score_rect = texte_score.get_rect(center=(largeur_fenetre // 2, hauteur_fenetre // 2 + 50))
    fenetre.blit(texte_score, texte_score_rect)

    # Afficher le bouton pour retourner au menu
    pygame.draw.rect(fenetre, VERT, (largeur_fenetre // 2 - 100, 400, 200, 50))
    texte_retour_menu = font.render("Retour au Menu", True, BLANC)
    texte_retour_menu_rect = texte_retour_menu.get_rect(center=(largeur_fenetre // 2, 425))
    fenetre.blit(texte_retour_menu, texte_retour_menu_rect)

    pygame.display.flip()

    return texte_retour_menu_rect

# Fonction principale
def main():
    global largeur_fenetre, hauteur_fenetre  # Déclaration de variables globales

    # Initialisation de la fenêtre
    fenetre = pygame.display.set_mode((largeur_fenetre, hauteur_fenetre), pygame.RESIZABLE)
    pygame.display.set_caption("Jeu de Tir")

    # Police de caractères
    font = pygame.font.Font(None, 36)

    # Gestion du jeu
    en_menu = True
    option_index = 0
    difficulty = None
    horloge = pygame.time.Clock()
    last_enemy_spawn_time = time.time()
    score = 0

    while en_menu:
        afficher_menu(fenetre, font, option_index)
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                pygame.quit()
                return
            elif event.type == pygame.KEYDOWN:
                if event.key == pygame.K_UP:
                    option_index = (option_index - 1) % len(difficulty_settings.keys())
                elif event.key == pygame.K_DOWN:
                    option_index = (option_index + 1) % len(difficulty_settings.keys())
                elif event.key == pygame.K_RETURN:
                    difficulty = list(difficulty_settings.keys())[option_index]
                    en_menu = False
            elif event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:  # Clic gauche de la souris
                mouse_x, mouse_y = pygame.mouse.get_pos()
                if retour_menu_rect.collidepoint(mouse_x, mouse_y):
                    en_menu = True

        # Redimensionner la fenêtre
        largeur_fenetre, hauteur_fenetre = fenetre.get_size()

    # Création des sprites
    tous_les_sprites = pygame.sprite.Group()
    projectiles = pygame.sprite.Group()
    ennemis = pygame.sprite.Group()
    vaisseau = Vaisseau((30, 30))
    tous_les_sprites.add(vaisseau)

    defaite = False

    while True:
        if not defaite:
            fenetre.fill(NOIR)

            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    return
                elif event.type == pygame.MOUSEBUTTONDOWN:
                    projectile = Projectile(vaisseau.rect.x + 50, vaisseau.rect.y + 10, (10, 5))
                    projectiles.add(projectile)
                    tous_les_sprites.add(projectile)
                elif event.type == pygame.VIDEORESIZE:
                    largeur_fenetre = event.w
                    hauteur_fenetre = event.h
                    fenetre = pygame.display.set_mode((largeur_fenetre, hauteur_fenetre), pygame.RESIZABLE)

            tous_les_sprites.update()

            # Vérifier les collisions entre les projectiles et les ennemis
            collisions = pygame.sprite.groupcollide(projectiles, ennemis, True, True)
            if collisions:
                score += 1

            # Vérifier les collisions entre les ennemis et le mur
            for ennemi in ennemis:
                if ennemi.rect.left <= 0:
                    defaite = True

            # Vérifier le temps écoulé depuis la dernière génération d'ennemi
            if time.time() - last_enemy_spawn_time >= difficulty_settings[difficulty]['enemy_spawn_interval']:
                spawn_enemy(ennemis, tous_les_sprites, (20, 20), difficulty_settings[difficulty]['enemy_speed'])
                last_enemy_spawn_time = time.time()

            # Adapter les éléments du jeu à la nouvelle taille de la fenêtre
            vaisseau.rect.y = min(vaisseau.rect.y, hauteur_fenetre - vaisseau.rect.height)
            for ennemi in ennemis:
                ennemi.rect.y = min(ennemi.rect.y, hauteur_fenetre - ennemi.rect.height)

            # Afficher le score
            texte_score = font.render("Score: {}".format(score), True, BLANC)
            texte_score_rect = texte_score.get_rect(topright=(largeur_fenetre - 10, 10))
            fenetre.blit(texte_score, texte_score_rect)

            tous_les_sprites.draw(fenetre)
            pygame.display.flip()

            horloge.tick(30)
        else:
            retour_menu_rect = afficher_defaite(fenetre, font, score)
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    pygame.quit()
                    return
                elif event.type == pygame.MOUSEBUTTONDOWN and event.button == 1:  # Clic gauche de la souris
                    mouse_x, mouse_y = pygame.mouse.get_pos()
                    if retour_menu_rect.collidepoint(mouse_x, mouse_y):
                        main()  # Retourner au menu

if __name__ == "__main__":
    difficulty_settings = {
        'Facile': {'enemy_speed': 4, 'enemy_spawn_interval': 3},
        'Moyen': {'enemy_speed': 5, 'enemy_spawn_interval': 2},
        'Difficile': {'enemy_speed': 6, 'enemy_spawn_interval': 1}
    }
    main()
