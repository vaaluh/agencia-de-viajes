# agencia-de-viajes
tarea 
## codigo
<?php
function generarNotificacion($mensaje, $tipo = "info") {

    $colores = [
        "info"   => "#2196F3",
        "exito"  => "#4CAF50",
        "alerta" => "#FFC107",
        "error"  => "#F44336"
    ];

    $color = $colores[$tipo] ?? $colores["info"];

    echo '
    <script>
    document.addEventListener("DOMContentLoaded", function() {
        let pop = document.createElement("div");
        pop.style.position = "fixed";
        pop.style.bottom = "20px";
        pop.style.right = "20px";
        pop.style.padding = "15px 20px";
        pop.style.background = "'.$color.'";
        pop.style.color = "white";
        pop.style.borderRadius = "10px";
        pop.style.fontFamily = "Arial";
        pop.style.boxShadow = "0px 4px 10px rgba(0,0,0,0.3)";
        pop.innerText = "'.$mensaje.'";

        document.body.appendChild(pop);

        setTimeout(() => {
            pop.style.opacity = "0";
            setTimeout(() => pop.remove(), 500);
        }, 4000);
    });
    </script>
    ';
}
?>

<?php
$ofertasdeviajes = [
    [
        "destino" => "Santiago",
        "fecha" => "2025-01-10",
        "precio" => 140000,
        "imagen" => "https://media.istockphoto.com/id/913781186/es/foto/horizonte-de-santiago-de-chile.jpg?s=2048x2048&w=is&k=20&c=0SFgsSqz11SC342qBr3Ouqq4fEdfm92yLmUZ_k2Jmeo=",
        "hoteles" => ["Hotel Plaza Santiago", "Hotel Intercontinental"]
    ],
    [
        "destino" => "Valparaíso",
        "fecha" => "2025-01-10",
        "precio" => 95000,
        "imagen" => "https://photo620x400.mnstatic.com/f52cf70bec1032fca3955131f4552b61/valparaiso.jpg",
        "hoteles" => ["Hotel Reina Victoria", "Hotel Fauna", "Casa Cervecera"]
    ],
    [
        "destino" => "Viña del Mar",
        "fecha" => "2025-01-11",
        "precio" => 120000,
        "imagen" => "https://chile.tur.com/_next/image?url=https%3A%2F%2Fd6myp1633h7qr.cloudfront.net%2Fposts-assets%2Fb260221f-4c13-4ead-892d-3a9e9ab8d4a7.jpeg&w=1200&q=75",
        "hoteles" => ["Hotel O'Higgins", "Hotel Enjoy", "Hotel San Martín"]
    ],
    [
        "destino" => "La Serena",
        "fecha" => "2025-02-05",
        "precio" => 170000,
        "imagen" => "https://viajexchile.com/wp-content/uploads/2017/11/1laserena.jpg",
        "hoteles" => ["Hotel Costa Real", "Hotel Francisco de Aguirre", "Hotel Canto del Mar"]
    ],
    [
        "destino" => "Puerto Montt",
        "fecha" => "2025-03-20",
        "precio" => 180000,
        "imagen" => "https://blog.trovit.cl/wp-content/uploads/2023/10/Puerto_Montt_5-1536x1152.jpeg",
        "hoteles" => ["Hotel Puerto Montt", "Hotel Manquehue"]
    ],
    [
        "destino" => "Cordillera de los Andes",
        "fecha" => "2025-01-18",
        "precio" => 450000,
        "imagen" => "https://humanidades.com/wp-content/uploads/2016/09/cordillera-de-los-andes-e1560489279388.jpg",
        "hoteles" => ["Lodge El Andino", "Hotel Cordillera Nevada"]
    ]
];

$resultados = [];

if (isset($_GET['destino']) || isset($_GET['fecha'])) {

    $busquedaDestino = strtolower($_GET['destino'] ?? "");
    $busquedaFecha = $_GET['fecha'] ?? "";

    foreach ($ofertasdeviajes as $oferta) {
        if (
            (empty($busquedaDestino) || strpos(strtolower($oferta["destino"]), $busquedaDestino) !== false) &&
            (empty($busquedaFecha) || $busquedaFecha == $oferta["fecha"])
        ) {
            $resultados[] = $oferta;
        }
    }
}
?>
<?php
if (isset($_GET['destino']) && isset($_GET['fecha'])) {

    
    $destinoIngresado = $_GET['destino'];
    $fechaIngresada = $_GET['fecha'];

    echo "<p style='color:green; font-size:18px;'>
             Planicacion de viaje registrada: <br>
            <strong>Destino:</strong> $destinoIngresado <br>
            <strong>Fecha:</strong> $fechaIngresada
          </p>";
}
?>
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Agencia de Viajes con PHP</title>

<style>
    body { font-family: Arial; }
    .result {
        border: 1px solid #ccc;
        padding: 10px;
        border-radius: 8px;
        width: 250px;
        margin: 10px;
        display: inline-block;
        vertical-align: top;
    }
</style>
</head>

<body>

<?php
generarNotificacion("🔥 ¡Oferta del día! 25% de descuento en vuelos nacionales", "exito");
?>

<h1 style="text-align:center; color:red;">Agencia de Viajes</h1>

<form method="GET">

    <label>Destino:</label>
    <input list="lista-destinos" name="destino" placeholder="Escribe un destino..."
        value="<?php echo $_GET['destino'] ?? ""; ?>">

    <datalist id="lista-destinos">
        <option value="Santiago">
        <option value="Valparaíso">
        <option value="Viña del Mar">
        <option value="La Serena">
        <option value="Puerto Montt">
        <option value="Cordillera de los Andes">
    </datalist>

    <br><br>

    <label>Fecha de viaje:</label>
    <input type="date" name="fecha" value="<?php echo $_GET['fecha'] ?? ""; ?>">

    <br><br>

    <button type="submit">Buscar</button>

</form>

<hr>

<div id="results-container">

<?php if (!empty($resultados)): ?>

    <?php foreach ($resultados as $r): ?>
        <div class="result">
            <img src="<?= $r['imagen'] ?>" width="230" style="border-radius:8px;"><br><br>

            <strong><?= $r['destino'] ?></strong><br>
            Fecha: <?= $r['fecha'] ?><br>
            Precio: $<?= $r['precio'] ?><br><br>

            <strong>Hoteles disponibles:</strong>
            <ul>
                <?php foreach ($r['hoteles'] as $h): ?>
                    <li><?= $h ?></li>
                <?php endforeach; ?>
            </ul>

            <button onclick="alert('Reserva generada con éxito')">Reservar</button>
        </div>
    <?php endforeach; ?>

<?php elseif (isset($_GET['destino']) || isset($_GET['fecha'])): ?>

    <p>No se encontraron resultados.</p>

<?php endif; ?>

</div>

</body>
</html>
