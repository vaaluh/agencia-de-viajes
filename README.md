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

##paquetesturisticos
$ofertasdeviajes = [
    [
        "destino" => "Santiago",
        "fecha" => "2025-01-10",
        "precio" => 140000,
        "imagen" => "https://media.istockphoto.com/id/913781186/es/foto/horizonte-de-santiago-de-chile.jpg",
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
        "imagen" => "https://chile.tur.com/_next/image?url=https%3A%2F%2Fd6myp1633h7qr.cloudfront.net%2Fposts-assets%2Fb260221f-4c13-4ead-892d-3a9e9ab8d4a7.jpeg",
        "hoteles" => ["Hotel O'Higgins", "Hotel Enjoy", "Hotel San Martín"]
    ],
    [
        "destino" => "La Serena",
        "fecha" => "2025-02-05",
        "precio" => 170000,
        "imagen" => "https://viajexchile.com/wp-content/uploads/2017/11/1laserena.jpg",
        "hoteles" => ["Hotel Costa Real", "Hotel Francisco de Aguirre", "Hotel Canto del Mar"]
    ]
];

##funcionvuelos
$vuelos = [
    ["origen"=>"Santiago","destino"=>"Valparaíso","fecha"=>"2025-01-10","precio"=>45000],
    ["origen"=>"Santiago","destino"=>"Viña del Mar","fecha"=>"2025-01-11","precio"=>55000],
    ["origen"=>"Santiago","destino"=>"La Serena","fecha"=>"2025-02-05","precio"=>85000],
    ["origen"=>"Santiago","destino"=>"Puerto Montt","fecha"=>"2025-03-20","precio"=>95000]
];

##busquedapaquetes
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

##busquedavuelos
$resultadosVuelos = [];

if (isset($_GET['origen']) || isset($_GET['destino_vuelo']) || isset($_GET['fecha_vuelo'])) {

    $origen = strtolower($_GET['origen'] ?? "");
    $destinoVuelo = strtolower($_GET['destino_vuelo'] ?? "");
    $fechaVuelo = $_GET['fecha_vuelo'] ?? "";

    foreach ($vuelos as $vuelo) {
        if (
            (empty($origen) || strpos(strtolower($vuelo["origen"]), $origen) !== false) &&
            (empty($destinoVuelo) || strpos(strtolower($vuelo["destino"]), $destinoVuelo) !== false) &&
            (empty($fechaVuelo) || $fechaVuelo == $vuelo["fecha"])
        ) {
            $resultadosVuelos[] = $vuelo;
        }
    }
}
?>

<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Agencia de Viajes</title>

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

<?php generarNotificacion("🔥 ¡Oferta del día! 25% de descuento en vuelos nacionales", "exito"); ?>

<h1 style="text-align:center; color:red;">Agencia de Viajes</h1>


<h2>Paquetes Turísticos</h2>

<form method="GET">
    <label>Destino:</label>
    <input list="lista-destinos" name="destino" value="<?= htmlspecialchars($_GET['destino'] ?? "") ?>">

    <label>Fecha:</label>
    <input type="date" name="fecha" value="<?= htmlspecialchars($_GET['fecha'] ?? "") ?>">

    <button type="submit">Buscar</button>
</form>

<hr>

<?php if (!empty($resultados)): ?>
    <?php foreach ($resultados as $r): ?>
        <div class="result">
            <img src="<?= $r['imagen'] ?>" width="230"><br><br>
            <strong><?= $r['destino'] ?></strong><br>
            Fecha: <?= $r['fecha'] ?><br>
            Precio: $<?= $r['precio'] ?><br>
            <ul>
                <?php foreach ($r['hoteles'] as $h): ?>
                    <li><?= $h ?></li>
                <?php endforeach; ?>
            </ul>
            <button onclick="alert('Paquete reservado con éxito')">Reservar</button>
        </div>
    <?php endforeach; ?>
<?php endif; ?>

<hr>

<h2>Vuelos</h2>

<form method="GET">
    <label>Origen:</label>
    <input list="lista-destinos" name="origen">

    <label>Destino:</label>
    <input list="lista-destinos" name="destino_vuelo">

    <label>Fecha:</label>
    <input type="date" name="fecha_vuelo">

    <button type="submit">Buscar vuelo</button>
</form>

<?php if (!empty($resultadosVuelos)): ?>
    <?php foreach ($resultadosVuelos as $v): ?>
        <div class="result">
            <strong><?= $v['origen'] ?> → <?= $v['destino'] ?></strong><br>
            Fecha: <?= $v['fecha'] ?><br>
            Precio: $<?= $v['precio'] ?><br><br>
            <button onclick="alert('Vuelo reservado con éxito')">Reservar vuelo</button>
        </div>
    <?php endforeach; ?>
<?php endif; ?>

<datalist id="lista-destinos">
    <option value="Santiago">
    <option value="Valparaíso">
    <option value="Viña del Mar">
    <option value="La Serena">
    <option value="Puerto Montt">
</datalist>

</body>
</html>
