<?php
// Paramètres de connexion à ta base Railway
$host = "ballast.proxy.rlwy.net";
$port = 56061;
$dbname = "railway";
$user = "root";
$pass = "xQdeoxrqUcymShCRvKkABvJAYrHKYsfy";

// Connexion MySQLi
$conn = new mysqli($host, $user, $pass, $dbname, $port);

// Vérification de la connexion
if ($conn->connect_error) {
    die("Connexion échouée : " . $conn->connect_error);
}

// Récupérer les données POST du formulaire
$username = $_POST['username'] ?? "";
$email = $_POST['email'] ?? "";
$password = $_POST['password'] ?? "";

if ($username && $email && $password) {
    // Hasher le mot de passe pour la sécurité
    $hashed_password = password_hash($password, PASSWORD_DEFAULT);

    // Préparer la requête SQL sécurisée
    $sql = "INSERT INTO users (username, email, password) VALUES (?, ?, ?)";
    $stmt = $conn->prepare($sql);
    $stmt->bind_param("sss", $username, $email, $hashed_password);

    // Exécuter et vérifier
    if ($stmt->execute()) {
        echo "✅ Utilisateur enregistré avec succès.";
    } else {
        echo "❌ Erreur lors de l'enregistrement : " . $stmt->error;
    }
    $stmt->close();
} else {
    echo "❌ Veuillez remplir tous les champs.";
}
$conn->close();
?>
