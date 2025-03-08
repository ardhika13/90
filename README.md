<?php
// Konfigurasi database
$host = "localhost";
$username = "root";
$password = "";
$database = "mydatabase"; // Pastikan nama database sudah benar

// Membuat koneksi ke MySQL
$conn = new mysqli($host, $username, $password, $database);

// Cek koneksi
if ($conn->connect_error) {
    die("Koneksi gagal: " . $conn->connect_error);
}

// Cek apakah form telah disubmit
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $waktu = $_POST["waktu"];
    $uraian = $_POST["uraian"];

    // Query untuk menambahkan data ke tabel kegiatan
    $sql = "INSERT INTO kegiatan (`Waktu`, `Uraian Kegiatan`) VALUES ('$waktu', '$uraian')";

    if ($conn->query($sql) === TRUE) {
        echo "<p class='success-message'>Data berhasil ditambahkan</p>";
    } else {
        echo "<p class='error-message'>Error: " . $sql . "<br>" . $conn->error . "</p>";
    }
}
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Manajemen Kegiatan</title>
    <style>
        /* Tambahkan CSS agar lebih menarik */
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }

        .container {
            max-width: 600px;
            margin: auto;
            background: white;
            padding: 20px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }

        h1 {
            text-align: center;
            color: #333;
        }

        label {
            display: block;
            margin-bottom: 8px;
            font-weight: bold;
        }

        input[type="text"] {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
        }

        button {
            padding: 10px 15px;
            background-color: #28a745;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        button:hover {
            background-color: #218838;
        }

        .success-message {
            color: green;
        }

        .error-message {
            color: red;
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>Manajemen Kegiatan</h1>

        <!-- Form untuk menambah kegiatan -->
        <form method="POST">
            <label for="waktu">Waktu Kegiatan</label>
            <input type="text" name="waktu" id="waktu" placeholder="Masukkan waktu kegiatan" required>

            <label for="uraian">Uraian Kegiatan</label>
            <input type="text" name="uraian" id="uraian" placeholder="Masukkan uraian kegiatan" required>

            <button type="submit">Tambah Kegiatan</button>
        </form>

        <h2>Daftar Kegiatan</h2>

        <?php
        // Query untuk menampilkan data dari tabel kegiatan
        $sql = "SELECT No, Waktu, `Uraian Kegiatan` FROM kegiatan";
        $result = $conn->query($sql);

        if ($result->num_rows > 0) {
            echo "<ul>";
            while($row = $result->fetch_assoc()) {
                echo "<li>No: " . $row["No"] . " | Waktu: " . $row["Waktu"] . " | Uraian: " . $row["Uraian Kegiatan"] . "</li>";
            }
            echo "</ul>";
        } else {
            echo "<p>Tidak ada data</p>";
        }

        // Menutup koneksi
        $conn->close();
        ?>
    </div>

</body>
</html>
