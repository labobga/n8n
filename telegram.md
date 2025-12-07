1. Recherchez le BotFather : Ouvrez Telegram et dans la barre de recherche, tapez @BotFather. Assurez-vous de sélectionner le compte vérifié (avec une coche bleue).

Démarrez la conversation : Envoyez la commande /start au BotFather.

2. Créer un Nouveau Bot

Pour obtenir un jeton, vous devez d'abord créer le bot auquel il sera associé.

    Lancez la création : Envoyez la commande /newbot.

    Nom d'affichage : Le BotFather vous demandera de choisir un nom affiché pour votre bot (Exemple : Mon Service de Notifications).

    Nom d'utilisateur (Username) : Le BotFather vous demandera ensuite de choisir un nom d'utilisateur unique pour votre bot. Ce nom doit se terminer par le mot bot (Exemple : mon_service_notifications_bot).


3. Récupérer l'Access Token

Une fois que vous avez réussi à nommer et enregistrer votre bot, le BotFather vous fournira immédiatement votre jeton d'accès.

    Le message de confirmation du BotFather contiendra une ligne qui ressemble à ceci :

        Done! Congratulations on your new bot. You will find it at t.me/
        votre_nom_utilisateur_bot

        . Use this token to access the HTTP API:

        123456789:ABC-DEF1234ghIkl-mnopQRStuvwxyZ


4. Chat ID : C'est le point de blocage le plus fréquent. Il vous faut l'ID numérique de l'utilisateur ou du groupe (et non le nom d'utilisateur @).

    💡 Comment obtenir votre Chat ID personnel :

        Recherchez le bot @userinfobot sur Telegram.

        Démarrez une conversation avec lui.

        Il vous donnera votre ID : ce sera un nombre négatif pour un groupe (Exemple : -123456789) ou un nombre positif pour un utilisateur (Exemple : 987654321). Utilisez ce nombre dans le champ Chat ID.
