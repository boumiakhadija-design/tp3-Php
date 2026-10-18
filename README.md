<?php
/**
 * Auteur : Étudiant
 * Objectif : Mini-application complète "Suivi des notes des apprenants" (Tous les exercices réunis)[cite: 1]
 * Date : 2026-10-18
 */

// Démarrage des sessions (Partie 6 - Exercice 6.1)[cite: 9]
session_start();

// ---------------------------------------------------------
// CONFIGURATION ET CONSTANTES (Partie 1 - Exercice 1.2)[cite: 3]
// ---------------------------------------------------------
const ETABLISSEMENT = "Institut Supérieur de Technologie";
define('NOTE_MIN', 0);
define('NOTE_MAX', 20);
define('SEUIL', 10);

// Création automatique du dossier data pour les fichiers CSV/JSON (Partie 0 & 5)[cite: 2, 8]
if (!is_dir('data')) {
    mkdir('data', 0777, true);
}

// Gestion de la navigation par onglets dans ce fichier unique
$section = $_GET['section'] ?? 'accueil';
?>
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title><?= ETABLISSEMENT ?> - TP PHP</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; background-color: #f4f6f9; color: #333; }
        header { background: #007bff; color: white; padding: 15px; border-radius: 5px; text-align: center; }
        nav { background: #343a40; padding: 10px; margin-top: 15px; border-radius: 5px; }
        nav a { color: white; margin-right: 15px; text-decoration: none; font-weight: bold; }
        nav a:hover { text-decoration: underline; }
        .container { background: white; padding: 20px; margin-top: 15px; border-radius: 5px; box-shadow: 0 2px 5px rgba(0,0,0,0.1); }
        table { border-collapse: collapse; width: 100%; margin-top: 10px; }
        th, td { border: 1px solid #dee2e6; padding: 10px; text-align: left; }
        th { background-color: #e9ecef; }
        .rouge { color: red; font-weight: bold; }
        .erreur { color: #dc3545; font-weight: bold; }
        .succes { color: #28a745; font-weight: bold; }
        fieldset { background: #fff; padding: 15px; margin-bottom: 15px; border: 1px solid #ced4da; border-radius: 4px; }
    </style>
</head>
<body>

    <header>
        <h1><?= ETABLISSEMENT ?></h1>
        <p>Mini-application : Suivi des notes des apprenants[cite: 1]</p>
    </header>

    <!-- Menu de navigation principal -->
    <nav>
        <a href="?section=accueil">Accueil</a>
        <a href="?section=partie1">1. Variables & Chaînes</a>
        <a href="?section=partie2">2. Flux & Boucles</a>
        <a href="?section=partie3">3. Tableaux & Stats</a>
        <a href="?section=partie4">4. Formulaires (GET/POST)</a>
        <a href="?section=partie5">5. Fichiers CSV & JSON</a>
        <a href="?section=partie6">6. Sessions, Cookies & Dates</a>
    </nav>

    <div class="container">
        <?php
        switch ($section) {
            case 'accueil':
                echo "<h2>Bienvenue</h2>";
                echo "<p>Ce fichier unique contient l'ensemble des exercices du TP 3 PHP demandé[cite: 1]. Utilisez le menu ci-dessus pour naviguer.</p>";
                break;

            // -------------------------------------------------
            // PARTIE 1 : Variables, constantes, types et chaînes
            // -------------------------------------------------
            case 'partie1':
                echo "<h2>Partie 1 : Variables, constantes, types et chaînes[cite: 3]</h2>";
                
                // Ex 1.3 : Calcul
                $prix_ht = 60;
                $tva = 20;
                $prix_ttc = $prix_ht * (1 + $tva / 100);
                echo "<p><b>Exercice 1.3 :</b> Le prix TTC du produit est de {$prix_ttc} €[cite: 3].</p>";

                // Ex 1.4 : Types et conversions
                $test = 42;
                $test_str = (string) $test;
                echo "<p><b>Exercice 1.4 :</b> Cast de 42 en string vérifié par var_dump : ";
                var_dump($test_str);
                echo "</p>";

                // Ex 1.5 : Chaînes de caractères
                $traitees = "égales à";
                $ici = 2;
                echo "<p><b>Exercice 1.5 :</b> Les variables seront $traitees $ici[cite: 4]</p>";
                $chaine_nettoyee = trim("*Bonjour les développeurs*", "*");
                echo "<p>Chaîne après trim() : $chaine_nettoyee[cite: 4]</p>";
                break;

            // -------------------------------------------------
            // PARTIE 2 : Structures de contrôle et boucles
            // -------------------------------------------------
            case 'partie2':
                echo "<h2>Partie 2 : Structures de contrôle et boucles[cite: 4, 5]</h2>";
                
                // Ex 2.1 : Fonction mention
                function mention_note($note) {
                    if ($note >= 16) return "Très bien[cite: 4]";
                    if ($note >= 14) return "Bien[cite: 4]";
                    if ($note >= 12) return "Assez bien[cite: 4]";
                    if ($note > SEUIL) return "Passable[cite: 4]";
                    return "Ajourné[cite: 4]";
                }
                echo "<p><b>Exercice 2.1 :</b> Mention pour une note de 15/20 : <b>" . mention_note(15) . "</b></p>";

                // Ex 2.2 : Switch sur le mois
                $mois = date("m");
                switch ($mois) {
                    case '10': $nom_mois = "Octobre"; break;
                    default: $nom_mois = "Mois en cours";
                }
                echo "<p><b>Exercice 2.2 :</b> Mois actuel : $nom_mois[cite: 4]</p>";

                // Ex 2.3 : Boucles (Triangle)
                echo "<p><b>Exercice 2.3 :</b> Affichage du triangle :</p><pre>";
                for ($i = 1; $i <= 5; $i++) {
                    echo str_repeat('*', $i) . "\n";
                }
                echo "</pre>";
                break;

            // -------------------------------------------------
            // PARTIE 3 : Tableaux
            // -------------------------------------------------
            case 'partie3':
                echo "<h2>Partie 3 : Tableaux et Statistiques[cite: 5, 6, 7]</h2>";
                
                $notes = [
                    "Mohamed" => 16, "Ahmed" => 14, "Hind" => 13, "Aicha" => 15,
                    "Samir" => 13, "Zakaria" => 13, "Rafik" => 10, "Ilham" => 9,
                    "Hamza" => 7, "Sami" => 7, "Noura" => 14
                ];

                // Affichage HTML avec condition de couleur si < SEUIL (Ex 3.2)[cite: 7]
                echo "<table><tr><th>Nom</th><th>Note</th><th>Mention</th></tr>";
                foreach ($notes as $nom => $note) {
                    $classe = ($note < SEUIL) ? "class='rouge'" : "";
                    $m = ($note >= 16) ? "Très bien" : (($note >= 14) ? "Bien" : (($note >= 12) ? "Assez bien" : (($note > SEUIL) ? "Passable" : "Ajourné")));
                    echo "<tr $classe><td>$nom</td><td>$note</td><td>$m</td></tr>";
                }
                echo "</table>";
                break;

            // -------------------------------------------------
            // PARTIE 4 : Formulaires (GET et POST)
            // -------------------------------------------------
            case 'partie4':
                echo "<h2>Partie 4 : Formulaires GET et POST[cite: 7, 8]</h2>";
                
                // Traitement GET (Ex 4.2)
                if (isset($_GET['nom_get'])) {
                    $n_get = htmlspecialchars($_GET['nom_get']);
                    $p_get = htmlspecialchars($_GET['prenom_get']);
                    $civ = ($_GET['sexe'] === 'M') ? "Monsieur" : "Madame";
                    echo "<p class='succes'>Bonjour $civ $p_get $n_get[cite: 7]</p>";
                }

                // Traitement POST (Ex 4.3 & 5.1)
                if ($_SERVER['REQUEST_METHOD'] === 'POST' && isset($_POST['nom_post'])) {
                    $nom_p = trim($_POST['nom_post']);
                    $ni = filter_var($_POST['note_info'], FILTER_VALIDATE_FLOAT);
                    $nm = filter_var($_POST['note_math'], FILTER_VALIDATE_FLOAT);

                    if (!empty($nom_p) && $ni !== false && $nm !== false && $ni >= NOTE_MIN && $ni <= NOTE_MAX && $nm >= NOTE_MIN && $nm <= NOTE_MAX) {
                        // Écriture dans le fichier CSV (Exercice 5.1)[cite: 8]
                        $fp = fopen('data/notes.csv', 'a');
                        fputcsv($fp, [$nom_p, $ni, $nm], ';');
                        fclose($fp);
                        echo "<p class='succes'>Enregistrement réussi pour " . htmlspecialchars($nom_p) . " (ajouté dans le fichier CSV) !</p>";
                    } else {
                        echo "<p class='erreur'>Erreur : Vérifiez que les notes sont comprises entre " . NOTE_MIN . " et " . NOTE_MAX . "[cite: 8].</p>";
                    }
                }
                ?>
                <fieldset>
                    <legend>Exercice 4.2 : Formulaire en GET</legend>
                    <form method="GET" action="">
                        <input type="hidden" name="section" value="partie4">
                        <label>Nom : <input type="text" name="nom_get" required></label>
                        <label>Prénom : <input type="text" name="prenom_get" required></label>
                        <label>Sexe : 
                            <select name="sexe">
                                <option value="M">Masculin</option>
                                <option value="F">Féminin</option>
                            </select>
                        </label>
                        <button type="submit">Envoyer (GET)</button>
                    </form>
                </fieldset>

                <fieldset>
                    <legend>Exercice 4.3 & 5.1 : Formulaire en POST (Saisie Notes)</legend>
                    <form method="POST" action="?section=partie4">
                        <label>Nom : <input type="text" name="nom_post" required></label>
                        <label>Note Info : <input type="number" step="0.5" name="note_info" required></label>
                        <label>Note Math : <input type="number" step="0.5" name="note_math" required></label>
                        <button type="submit">Enregistrer (POST)</button>
                    </form>
                </fieldset>
                <?php
                break;

            // -------------------------------------------------
            // PARTIE 5 : Fichiers CSV et JSON
            // -------------------------------------------------
            case 'partie5':
                echo "<h2>Partie 5 : Fichiers CSV et JSON[cite: 8, 9]</h2>";
                
                // Lecture CSV (Ex 5.2)[cite: 8]
                $fichier = 'data/notes.csv';
                if (file_exists($fichier)) {
                    $fp = fopen($fichier, 'r');
                    echo "<h3>Lecture du fichier CSV :</h3>";
                    echo "<table><tr><th>Nom</th><th>Info</th><th>Math</th></tr>";
                    while (($ligne = fgetcsv($fp, 1000, ';')) !== FALSE) {
                        echo "<tr><td>" . htmlspecialchars($ligne[0]) . "</td><td>{$ligne[1]}</td><td>{$ligne[2]}</td></tr>";
                    }
                    fclose($fp);
                    echo "</table>";
                } else {
                    echo "<p>Aucune donnée CSV disponible. Veuillez enregistrer des notes via la Partie 4.</p>";
                }

                // Sérialisation JSON (Ex 5.4)[cite: 9]
                $data_json = ["Mohamed" => 16, "Ahmed" => 14];
                $json_str = json_encode($data_json, JSON_PRETTY_PRINT);
                file_put_contents('data/notes.json', $json_str);
                echo "<h3>Exercice 5.4 : Export JSON</h3>";
                echo "<pre>" . htmlspecialchars($json_str) . "</pre>";
                break;

            // -------------------------------------------------
            // PARTIE 6 : Cookies, sessions, redirection et dates
            // -------------------------------------------------
            case 'partie6':
                echo "<h2>Partie 6 : Cookies, Sessions et Dates[cite: 9, 10]</h2>";
                
                // Gestion de la déconnexion et de la connexion (Ex 6.1 & 6.2)[cite: 9, 10]
                if (isset($_POST['logout'])) {
                    session_unset();
                    session_destroy();
                    header("Location: ?section=partie6");
                    exit;
                }
                if (isset($_POST['login_submit'])) {
                    $l = trim($_POST['login_val']);
                    if (!empty($l)) {
                        $_SESSION['login'] = $l;
                        setcookie("dernier_login", $l, time() + 86400, "/"); // Cookie 24h[cite: 10]
                    }
                }

                if (isset($_SESSION['login'])) {
                    echo "<p class='succes'>Bienvenue, " . htmlspecialchars($_SESSION['login']) . " (Session active) !</p>";
                    echo '<form method="POST"><button type="submit" name="logout">Se déconnecter</button></form>';
                } else {
                    $cookie_val = $_COOKIE['dernier_login'] ?? '';
                    ?>
                    <fieldset>
                        <legend>Connexion (Session & Cookie)</legend>
                        <form method="POST">
                            <label>Login : <input type="text" name="login_val" value="<?= htmlspecialchars($cookie_val) ?>" required></label>
                            <button type="submit" name="login_submit">Se connecter</button>
                        </form>
                    </fieldset>
                    <?php
                }
                
                // Gestion des Dates (Ex 6.3)[cite: 10]
                $date_jour = date('d/m/Y');
                $fin_semestre = new DateTime('2027-01-31');
                $aujourd_hui = new DateTime();
                $diff = $aujourd_hui->diff($fin_semestre);
                echo "<hr><p><b>Exercice 6.3 (Dates) :</b> Date du jour : $date_jour | Jours restants avant la fin du semestre : " . $diff->format('%r%a') . " jours[cite: 10].</p>";
                break;

            default:
                echo "<p class='erreur'>Section introuvable.</p>";
        }
        ?>
    </div>

    <footer style="text-align: center; margin-top: 20px; font-size: 0.9em; color: #6c757d;">
        <p>&copy; 2026 - <?= ETABLISSEMENT ?>[cite: 1]</p>
    </footer>
</body>
</html>
