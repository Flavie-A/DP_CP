Afin d’encapsuler tous les éléments nécessaires au bon fonctionnement de mon application dans un environnement de développement local sans avoir à se soucier d’éventuels soucis de dépendances, j’ai procédé à sa « dockerisation ». J’ai donc installé Docker Desktop pour Windows sur mon ordinateur, puis créé le fichier Dockerfile.dev à la racine de mon projet. Je l’ai alors réadapté à partir d’un modèle proposé par l’école et notamment sur ce tutoriel :
    https://www.frontendundefined.com/posts/tutorials/docker-react-development-environment/

    https://www.frontendundefined.com/posts/tutorials/docker-react-development-environment/
    # Utilisation de l'image de base Ubuntu
    FROM node:20
    # Définit le répertoire de l'image
    WORKDIR /app-meteo-dev
    # Copie les fichiers
    COPY package*.json .
    COPY . . .
    # Installe les dépendances de l'application
    RUN npm install
    # Configure le port que Vite utilise
    EXPOSE 1573
    # Démarre le serveur de développement
    CMD ["npm", "run", "dev"]
J’ai également créé le fichier .dockerignore pour ne pas transférer certains contenus comme par exemple les fichiers relatifs à Git ou le dossier node_modules (dont les contenus sont spécifiques à l’environnement local).
J’ai ensuite lancé la création de l’image app-meteo-dev avec la commande « docker build -t react-app -f Dockerfile.dev . » puis ai pu monter l’image avec la commande « docker run -v $(pwd):/app -p 5173:5173 app-meteo-dev » pour afficher immédiatement les évolutions à venir sans devoir reconstruire l'image. Grâce à ce fonctionnement, un travail collaboratif ou sur plusieurs postes serait également facilité en évitant les risques de dysfonctionnements liés aux différences de postes de travail ou d’environnement.
L’outil Git a permis de gérer les évolutions du code de l'application grâce à la traçabilité qu’il offre. Grâce aux commits réguliers réalisés dans la branche « main », les modifications apportées au code source (lors de l’installation du modèle, ou suite à la configuration de l’API par exemple) ont été mémorisées sous forme de versions clairement identifiables grâce aux commandes : « git add », « git commit -m "nom du commit" » puis « git push ». Ci-dessous des exemples de commits réalisés sur la branche master :
    flavie-a@FLAVIE-HP:~/react/oclock/meteo/Spe-React-Meteo-Widget--Flavie-A$ git log --oneline
    cca1fe6 (HEAD -> master, origin/master, origin/HEAD) fix:build for dockerfile
    b2b183c build: docker configuration
    e12f0f7 fix:correction API with thunk
    80cba8f feat:fetch API ok
    a2e76ac feat:add + delete city ok
    0825012 fix:installation files
    51011ec install ok
    81a8d98 Install React-modele-vite
    26063c3 Initial commit
