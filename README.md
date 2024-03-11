<!DOCTYPE html>
<html lang="fr">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    
    <title>Calculatrice</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.9.3/html2pdf.bundle.min.js"></script>

    <style>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background: linear-gradient(rgba(63, 66, 225, 0.5), #f32f50), linear-gradient(#3126f9, #f32f50);
    background-size: 200% 200%;
    animation: moveColors 5s linear infinite alternate;
}

@keyframes moveColors {
    0% {
        background-position: 0% 0%, 0% 0%;
    }
    100% {
        background-position: 100% 100%, 100% 100%;
    }
}

    
        

        #overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(3, 3, 3, 0.5);
            z-index: -1;
        }

        h1,
        h2 {
            color: #333;
            text-align: center;
            margin-bottom: 20px;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .container {
            width: 100%;
            max-width: 2600px;
            margin: 0 auto;
        }

        table {
            border-collapse: collapse;
            width: 100%;
            background-color: #fff;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }

        th,
        td {
            border: 1px solid #dddddd;
            text-align: center;
            padding: 5px;
        }

        th {
            background-color: #f2f2f2;
        }

        .input-column {
            width: 20%;
        }

        input[type="text"] {
            width: calc(100% - 10px);
            padding: 8px;
            box-sizing: border-box;
            border-radius: 4px;
            border: 1px solid #ccc;
            font-size: 14px;
            margin: 5px 5px;
        }

        .calculator-button {
            width: 100%;
            height: 50px;
            font-size: 16px;
            margin: 5px;
            cursor: pointer;
            background-color: #4d4e97;
            color: #ffffff;
            border: none;
            border-radius: 6px;
            transition: background-color 0.3s ease;
        }

        .calculator-button:hover {
            background-color: #d7475f;
        }

        #resultat_protégée {
            background-color: #f2f2f2;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        #resultat_zone {
            background-color: #f2f2f2;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .section-title {
            text-align: center;
            font-weight: bold;
            font-size: 24px;
            margin-bottom: 10px;
            color: #070707;
            background-color: #fff;
            display: inline-block;
            border: 2px solid #020202;
            padding: 10px 20px;
            border-radius: 8px;
        }

        #logo {
            max-width: 150px;
            display: block;
            margin: 0 auto;
            margin-top: 50px;
            margin-bottom: 20px;
            animation: moveLogo 3s infinite alternate;
        }

        @keyframes moveLogo {
            0% {
                transform: translateY(0);
            }

            100% {
                transform: translateY(-20px);
            }
        }

        .result-label {
            font-weight: bold;
        }

        .result-value {
            margin-bottom: 10px;
        }

        #resultat_porter_resistives {
    background-color: #fff; /* Fond blanc */
    padding: 20px;
    margin-bottom: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
.footer{
    height: 100px;
    text-align: center;
    color: #000000;
    background-color: #fffefe;
}

body {
            margin: 0;
            font-family: Arial, sans-serif;
        }

        .content {
            padding: 20px;
            text-align: center;
        }

        footer {
            background-color: #010101;
            color: #fff;
            padding: 20px 0;
            text-align: center;
        }

        .footer-content p {
            margin: 5px 0;
        }

        .footer-content a {
            color: #fff;
            text-decoration: none;
        }

        .footer-content a:hover {
            text-decoration: underline;
        }
    </style>

    
    <script>
        function calculer() {
            var longueur_aval = parseFloat(document.getElementById("longueur_aval").value.replace(",", "."));
            var xd_aval = parseFloat(document.getElementById("xd_aval").value.replace(",", "."));
            var rd_aval = parseFloat(document.getElementById("rd_aval").value.replace(",", "."));
            var xo_aval = parseFloat(document.getElementById("xo_aval").value.replace(",", "."));
            var ro_aval = parseFloat(document.getElementById("ro_aval").value.replace(",", "."));

            // Calcul de l'impédance directe Z dans la ligne aval protégée
            var imp_direct_aval = longueur_aval * Math.sqrt(Math.pow(rd_aval, 2) + Math.pow(xd_aval, 2));
            var imp_direct_aval_ohm = imp_direct_aval.toFixed(2) + " Ohm";

            // Calcul de l'angle en degrés
            var angle = (Math.atan(xd_aval / rd_aval)) * (180 / Math.PI);
            var angle_degrees = angle.toFixed(2) + " degrés";

            // Calcul du facteur de terre kZ0 Amplitude
            var facteur_terre_amplitude = Math.sqrt(Math.pow(ro_aval - rd_aval, 2) + Math.pow(xo_aval - xd_aval, 2)) / (Math.sqrt(Math.pow(rd_aval, 2) + Math.pow(xd_aval, 2)) * 3);
            var facteur_terre_amplitude_formatted = facteur_terre_amplitude.toFixed(2) + " Ohm";

            // Calcul du facteur de terre kZ0 Angle
            var facteur_terre_angle = (Math.atan((xo_aval - xd_aval) / (ro_aval - rd_aval)) - Math.atan(xd_aval / rd_aval)) * (180 / Math.PI);
            var facteur_terre_angle_formatted = facteur_terre_angle.toFixed(4) + " degrés";

            // Affichage des résultats
            document.getElementById("resultat_global_impedance").innerHTML = "<span class='result-label'>Impédance Primaire:</span> " + imp_direct_aval_ohm;
            document.getElementById("resultat_global_angle").innerHTML = "<span class='result-label'>Argument:</span> " + angle_degrees;
            document.getElementById("resultat_global_terre_kZ0_amplitude").innerHTML = "<span class='result-label'>Facteur de Terre kZ0 Amplitude:</span> " + facteur_terre_amplitude_formatted;
            document.getElementById("resultat_global_terre_kZ0_angle").innerHTML = "<span class='result-label'>Facteur de Terre kZ0 Angle:</span> " + facteur_terre_angle_formatted;

            // Calcul du rapport TT/TC
            calculerRapportTTTC();

            // Calcul de l'impédance de la zone 1 primaire
            calculerImpedanceZone1();

            // Calcul de l'impédance de la zone 2 primaire
            calculerImpedanceZone2();

            calculerImpedanceZone3();

            calculerImpedanceZone3Secondaire();

            calculerImpedanceZone4();

            calculerImpedanceZone4Secondaire();

            calculerChargeMax();

            calculerPorteeTerre();

            calculerPorteePhase();

        }



        function calculerRapportTTTC() {
            var imp_direct_aval = parseFloat(document.getElementById("resultat_global_impedance").innerText.split(":")[1]);
            var rapport_tt = parseFloat(document.getElementById("rapport_tt").value.replace(",", "."));
            var rapport_tc = parseFloat(document.getElementById("rapport_tc").value.replace(",", "."));

            // Vérifier si les rapports TT et TC sont valides
            if (!isNaN(rapport_tt) && !isNaN(rapport_tc) && rapport_tc !== 0) {
                // Calcul de l'impédance secondaire
                var imp_secondaire = imp_direct_aval / (rapport_tt / rapport_tc);
                document.getElementById("resultat_global_rapport").innerHTML = "<span class='result-label'>Impédance Secondaire:</span> " + imp_secondaire.toFixed(2) + " Ohm";
            } 
        }

        function calculerImpedanceZone1() {
            var imp_direct_aval = parseFloat(document.getElementById("resultat_global_impedance").innerText.split(":")[1]);
            var zone1_percentage = parseFloat(document.getElementById("z1").value.replace(",", "."));

            // Vérifier si le pourcentage de la zone 1 est valide
            if (!isNaN(zone1_percentage)) {
                // Calcul de l'impédance de la zone 1 primaire
                var imp_zone1_primaire = (zone1_percentage / 100) * imp_direct_aval;
                document.getElementById("resultat_zone1_primaire").innerHTML = "<span class='result-label'>Impédance Zone 1 Primaire:</span> " + imp_zone1_primaire.toFixed(2) + " Ohm";

                // Calcul de l'impédance de la zone 1 secondaire
                var rapport_tt = parseFloat(document.getElementById("rapport_tt").value.replace(",", "."));
                var rapport_tc = parseFloat(document.getElementById("rapport_tc").value.replace(",", "."));
                // Vérifier si les rapports TT et TC sont valides
                if (!isNaN(rapport_tt) && !isNaN(rapport_tc) && rapport_tc !== 0) {
                    var imp_zone1_secondaire = imp_zone1_primaire / (rapport_tt / rapport_tc);
                    document.getElementById("resultat_zone1_secondaire").innerHTML = "<span class='result-label'>Impédance Zone 1 Secondaire:</span> " + imp_zone1_secondaire.toFixed(2) + " Ohm";
                } 
            } 
        }


        function calculerImpedanceZone2() {
            var imp_direct_aval = parseFloat(document.getElementById("resultat_global_impedance").innerText.split(":")[1]);
            var zone2_percentage = parseFloat(document.getElementById("z2").value.replace(",", "."));

            // Vérifier si le pourcentage de la zone 2 est valide
            if (!isNaN(zone2_percentage)) {
                // Calcul de l'impédance de la zone 2 primaire
                var imp_zone2_primaire = (zone2_percentage / 100) * imp_direct_aval;
                document.getElementById("resultat_zone2_primaire").innerHTML = "<span class='result-label'>Impédance Zone 2 Primaire:</span> " + imp_zone2_primaire.toFixed(2) + " Ohm";

                // Calcul de l'impédance de la zone 2 secondaire
                var rapport_tt = parseFloat(document.getElementById("rapport_tt").value.replace(",", "."));
                var rapport_tc = parseFloat(document.getElementById("rapport_tc").value.replace(",", "."));

                // Vérifier si les rapports TT et TC sont valides
                if (!isNaN(rapport_tt) && !isNaN(rapport_tc) && rapport_tc !== 0) {
                    var imp_zone2_secondaire = imp_zone2_primaire / (rapport_tt / rapport_tc);
                    document.getElementById("resultat_zone2_secondaire").innerHTML = "<span class='result-label'>Impédance Zone 2 Secondaire:</span> " + imp_zone2_secondaire.toFixed(2) + " Ohm";
                } 
            }
        }
        function calculerImpedanceZone3() {
    var imp_direct_aval = parseFloat(document.getElementById("resultat_global_impedance").innerText.split(":")[1]);
    var longueur_adj = parseFloat(document.getElementById("longueur_adj").value.replace(",", "."));
    var xd_adj = parseFloat(document.getElementById("xd_adj").value.replace(",", "."));
    var rd_adj = parseFloat(document.getElementById("rd_adj").value.replace(",", "."));
    var zone3_percentage = parseFloat(document.getElementById("z3").value.replace(",", "."));

    // Vérifier si le pourcentage de la zone 3 est valide
    if (!isNaN(zone3_percentage)) {
        // Calcul de l'impédance de la zone 3 primaire
        var imp_zone3_primaire = (zone3_percentage / 100) * (imp_direct_aval + Math.sqrt(Math.pow(xd_adj, 2) + Math.pow(rd_adj, 2)) * longueur_adj);
        document.getElementById("resultat_zone3_primaire").innerHTML = "<span class='result-label'>Impédance Zone 3 Primaire:</span> " + imp_zone3_primaire.toFixed(2) + " Ohm";
    }
}
function calculerImpedanceZone3Secondaire() {
    var imp_zone3_primaire = parseFloat(document.getElementById("resultat_zone3_primaire").innerText.split(":")[1]);
    var rapport_tt = parseFloat(document.getElementById("rapport_tt").value.replace(",", "."));
    var rapport_tc = parseFloat(document.getElementById("rapport_tc").value.replace(",", "."));

    // Vérifier si les rapports TT et TC sont valides
    if (!isNaN(rapport_tt) && !isNaN(rapport_tc) && rapport_tc !== 0) {
        var imp_zone3_secondaire = imp_zone3_primaire / (rapport_tt / rapport_tc);
        document.getElementById("resultat_zone3_secondaire").innerHTML = "<span class='result-label'>Impédance Zone 3 Secondaire:</span> " + imp_zone3_secondaire.toFixed(2) + " Ohm";
    } else {
        document.getElementById("resultat_zone3_secondaire").innerHTML = "<span class='result-label'>Impédance Zone 3 Secondaire:</span> Données insuffisantes pour le calcul";
    }
}

function calculerImpedanceZone4() {
    var longueur_amo = parseFloat(document.getElementById("longueur_amo").value.replace(",", "."));
    var xd_amo = parseFloat(document.getElementById("xd_amo").value.replace(",", "."));
    var rd_amo = parseFloat(document.getElementById("rd_amo").value.replace(",", "."));
    var zone4_percentage = parseFloat(document.getElementById("z4").value.replace(",", "."));

    // Vérifier si le pourcentage de la zone 4 est valide
    if (!isNaN(zone4_percentage)) {
        // Calcul de l'impédance de la zone 4 primaire
        var imp_zone4_primaire = (zone4_percentage / 100) * (longueur_amo * Math.sqrt(Math.pow(rd_amo, 2) + Math.pow(xd_amo, 2)));
        document.getElementById("resultat_zone4_primaire").innerHTML = "<span class='result-label'>Impédance Zone 4 Primaire:</span> " + imp_zone4_primaire.toFixed(2) + " Ohm";

    }
}

function calculerImpedanceZone4Secondaire() {
    var imp_zone4_primaire = parseFloat(document.getElementById("resultat_zone4_primaire").innerText.split(":")[1]);
    var rapport_tt = parseFloat(document.getElementById("rapport_tt").value.replace(",", "."));
    var rapport_tc = parseFloat(document.getElementById("rapport_tc").value.replace(",", "."));

    // Vérifier si les rapports TT et TC sont valides
    if (!isNaN(rapport_tt) && !isNaN(rapport_tc) && rapport_tc !== 0) {
        var imp_zone4_secondaire = imp_zone4_primaire / (rapport_tt / rapport_tc);
        document.getElementById("resultat_zone4_secondaire").innerHTML = "<span class='result-label'>Impédance Zone 4 Secondaire:</span> " + imp_zone4_secondaire.toFixed(2) + " Ohm";
    } else {
        document.getElementById("resultat_zone4_secondaire").innerHTML = "<span class='result-label'>Impédance Zone 4 Secondaire:</span> Données insuffisantes pour le calcul";
    }
}



        function effacerChamps() {
            // Réinitialiser tous les champs de texte à leur valeur par défaut
            var inputs = document.querySelectorAll('input[type="text"]');
            inputs.forEach(function(input) {
                input.value = "";
            });

            // Effacer les résultats
            document.getElementById("resultat_global_impedance").innerHTML = "";
            document.getElementById("resultat_global_rapport").innerHTML = "";
            document.getElementById("resultat_global_angle").innerHTML = "";
            document.getElementById("resultat_global_terre_kZ0_amplitude").innerHTML = "";
            document.getElementById("resultat_global_terre_kZ0_angle").innerHTML = "";
            document.getElementById("resultat_zone1_primaire").innerHTML = "";
            document.getElementById("resultat_zone2_primaire").innerHTML = "";
            document.getElementById("resultat_zone2_secondaire").innerHTML = "";

            document.getElementById("resultat_zone1_primaire").innerHTML = "";
            document.getElementById("resultat_zone1_secondaire").innerHTML = "";
            document.getElementById("resultat_zone2_primaire").innerHTML = "";
            document.getElementById("resultat_zone2_secondaire").innerHTML = "";
            document.getElementById("resultat_zone3_primaire").innerHTML = "";
            document.getElementById("resultat_zone3_secondaire").innerHTML = "";
            document.getElementById("resultat_zone4_primaire").innerHTML = "";
            document.getElementById("resultat_zone4_secondaire").innerHTML = "";

            document.getElementById("resultat_charge_max").innerHTML = "";
            document.getElementById("resultat_PorteeTerre").innerHTML = "";
            document.getElementById("resultat_portee_phase").innerHTML = "";
               
        }

        // Fonction pour calculer Z Charge Max
function calculerChargeMax() {
    // Récupérer les valeurs de Vmin et Ith entrées par l'utilisateur
    var vmin = parseFloat(document.getElementById("vmin").value.replace(",", "."));
    var ith = parseFloat(document.getElementById("ith").value.replace(",", "."));

    // Vérifier si les valeurs sont valides
    if (!isNaN(vmin) && !isNaN(ith) && ith !== 0) {
        // Calculer Z Charge Max
        var zChargeMax = vmin / ith;

        // Afficher le résultat calculé dans l'élément HTML correspondant
        document.getElementById("resultat_charge_max").innerHTML = "<span class='result-label'>Z Charge Max:</span> " + zChargeMax.toFixed(2) + " Ohm";
    } else {
        // Si les valeurs ne sont pas valides, afficher un message d'erreur
        document.getElementById("resultat_charge_max").innerHTML = "<span class='result-label'>Z Charge Max:</span> Données insuffisantes pour le calcul";
    }
}

function calculerPorteeTerre() {
    // Récupérer le résultat de Z Charge Max
    var zChargeMaxElement = document.getElementById("resultat_charge_max");
    if (zChargeMaxElement) {
        var zChargeMaxText = zChargeMaxElement.innerText;
        var zChargeMaxValue = parseFloat(zChargeMaxText.split(":")[1]);

        // Si le résultat de Z Charge Max est valide, calculer et afficher la portée de terre
        if (!isNaN(zChargeMaxValue)) {
            var porteeTerre = 0.8 * zChargeMaxValue;
            document.getElementById("resultat_PorteeTerre").innerHTML = "<span class='result-label'>Portée de Terre:</span> " + porteeTerre.toFixed(2) + " Ohm";
        } else {
            // Si le résultat de Z Charge Max n'est pas valide, afficher un message d'erreur
            document.getElementById("resultat_PorteeTerre").innerHTML = "<span class='result-label'>Portée de Terre:</span> Données insuffisantes pour le calcul";
        }
    } else {
        // Si l'élément pour Z Charge Max n'est pas trouvé, afficher un message d'erreur
        document.getElementById("resultat_PorteeTerre").innerHTML = "<span class='result-label'>Portée de Terre:</span> Erreur de récupération des données";
    }
}

function calculerPorteePhase() {
    // Récupérer le résultat de Z Charge Max
    var zChargeMaxElement = document.getElementById("resultat_charge_max");
    if (zChargeMaxElement) {
        var zChargeMaxText = zChargeMaxElement.innerText;
        var zChargeMaxValue = parseFloat(zChargeMaxText.split(":")[1]);

        // Si le résultat de Z Charge Max est valide, calculer et afficher la portée de phase
        if (!isNaN(zChargeMaxValue)) {
            var porteePhase = 1.2 * zChargeMaxValue;
            document.getElementById("resultat_portee_phase").innerHTML = "<span class='result-label'>Portée de Phase:</span> " + porteePhase.toFixed(2) + " Ohm";
        } else {
            // Si le résultat de Z Charge Max n'est pas valide, afficher un message d'erreur
            document.getElementById("resultat_portee_phase").innerHTML = "<span class='result-label'>Portée de Phase:</span> Données insuffisantes pour le calcul";
        }
    } else {
        // Si l'élément pour Z Charge Max n'est pas trouvé, afficher un message d'erreur
        document.getElementById("resultat_portee_phase").innerHTML = "<span class='result-label'>Portée de Phase:</span> Erreur de récupération des données";
    }
}



    </script>
</head>

<body>
    <!-- Overlay pour l'opacité de l'image de fond -->
    <div id="overlay"></div>

    <!-- En-tête avec le logo -->
    <header>
        <img src="logo.png" alt="Logo de l'entreprise" id="logo">
    </header>

    <!-- Contenu principal -->
    <main>
        <h1 style="color: #fff;">Calculatrice Électrique</h1>

        <div class="container">
           

<div style="width: 800px; float: left; ">
     <!-- Ligne Aval Protégée -->
     <h2 class="section-title">Ligne Aval Protégée</h2>
     <!-- Ligne Aval Protégée (Haut de page) -->
    <table>
        <tr>
            <th class="input-column">Longueur</th>
            <th class="input-column">Xd</th>
            <th class="input-column">Rd</th>
            <th class="input-column">Xo</th>
            <th class="input-column">Ro</th>
        </tr>
        <tr>
            <td><input type="text" id="longueur_aval" name="longueur_aval"></td>
            <td><input type="text" id="xd_aval" name="xd_aval"></td>
            <td><input type="text" id="rd_aval" name="rd_aval"></td>
            <td><input type="text" id="xo_aval" name="xo_aval"></td>
            <td><input type="text" id="ro_aval" name="ro_aval"></td>
        </tr>
    </table>

<!-- Ligne Aval Protégée (Ensemble Bas de Page) -->
<fieldset>
    <legend  style="color: white;">Ligne Aval Protégée (Données2)</legend>
    <table>
        <tr>
            <th class="input-column">Longueur</th>
            <th class="input-column">Xd</th>
            <th class="input-column">Rd</th>
            <th class="input-column">Xo</th>
            <th class="input-column">Ro</th>
        </tr>
        <tr>
            <td><input type="text" id="longueur_aval_bottom" name="longueur_aval_bottom"></td>
            <td><input type="text" id="xd_aval_bottom" name="xd_aval_bottom"></td>
            <td><input type="text" id="rd_aval_bottom" name="rd_aval_bottom"></td>
            <td><input type="text" id="xo_aval_bottom" name="xo_aval_bottom"></td>
            <td><input type="text" id="ro_aval_bottom" name="ro_aval_bottom"></td>
        </tr>
    </table>
</fieldset>

</div>
<div style="width: 800px;  float: right;">

            <!-- Ligne Adjacente Aval -->
            <h2 class="section-title">Ligne Aval Adjacente la plus Protégée</h2>
            <table>
                <tr>
                    <th class="input-column">Longueur</th>
                    <th class="input-column">Xd</th>
                    <th class="input-column">Rd</th>
                    <th class="input-column">Xo</th>
                    <th class="input-column">Ro</th>
                </tr>
                <tr>
                    <td><input type="text" id="longueur_adj" name="longueur_adj"></td>
                    <td><input type="text" id="xd_adj" name="xd_adj"></td>
                    <td><input type="text" id="rd_adj" name="rd_adj"></td>
                    <td><input type="text" id="xo_adj" name="xo_adj"></td>
                    <td><input type="text" id="ro_adj" name="ro_adj"></td>
                </tr>
            </table>
 
            <!-- Ligne Aval Protégée (Ensemble Bas de Page) -->
<fieldset>
    <legend  style="color: white;">Ligne Aval Adjacente (Données2)</legend>
    <table>
        <tr>
            <th class="input-column">Longueur</th>
            <th class="input-column">Xd</th>
            <th class="input-column">Rd</th>
            <th class="input-column">Xo</th>
            <th class="input-column">Ro</th>
        </tr>
        <tr>
            <td><input type="text" id="longueur_aval_bottom" name="longueur_aval_bottom"></td>
            <td><input type="text" id="xd_aval_bottom" name="xd_aval_bottom"></td>
            <td><input type="text" id="rd_aval_bottom" name="rd_aval_bottom"></td>
            <td><input type="text" id="xo_aval_bottom" name="xo_aval_bottom"></td>
            <td><input type="text" id="ro_aval_bottom" name="ro_aval_bottom"></td>
        </tr>
    </table>
</fieldset>
</div>
<div style="width: 800px; margin-left: 900px; margin-bottom: 50px;">
            <!-- Ligne Amont la plus impédante Protégée -->
            <h2 class="section-title">Ligne Amont la plus impédante Protégée</h2>
            <table>
                <tr>
                    <th class="input-column">Longueur</th>
                    <th class="input-column">Xd</th>
                    <th class="input-column">Rd</th>
                    <th class="input-column">Xo</th>
                    <th class="input-column">Ro</th>
                </tr>
                <tr>
                    <td><input type="text" id="longueur_amo" name="longueur_amo"></td>
                    <td><input type="text" id="xd_amo" name="xd_amo"></td>
                    <td><input type="text" id="rd_amo" name="rd_amo"></td>
                    <td><input type="text" id="xo_amo" name="xo_amo"></td>
                    <td><input type="text" id="ro_amo" name="ro_amo"></td>
                </tr>
            </table>

            <fieldset>
                <legend style="color: white;">Ligne Aval Adjacente (Données2)</legend>
                <table>
                    <tr>
                        <th class="input-column">Longueur</th>
                        <th class="input-column">Xd</th>
                        <th class="input-column">Rd</th>
                        <th class="input-column">Xo</th>
                        <th class="input-column">Ro</th>
                    </tr>
                    <tr>
                        <td><input type="text" id="longueur_aval_bottom" name="longueur_aval_bottom"></td>
                        <td><input type="text" id="xd_aval_bottom" name="xd_aval_bottom"></td>
                        <td><input type="text" id="rd_aval_bottom" name="rd_aval_bottom"></td>
                        <td><input type="text" id="xo_aval_bottom" name="xo_aval_bottom"></td>
                        <td><input type="text" id="ro_aval_bottom" name="ro_aval_bottom"></td>
                    </tr>
                </table>
            </fieldset>
        </div>
        <div style="width: 800px; float: left;">
            <!-- Portée des Zones -->
            <h2 class="section-title">Portée des Zones</h2>
            <table>
                <tr>
                    <th class="input-column">Zone 1</th>
                    <th class="input-column">Zone 2</th>
                    <th class="input-column">Zone 3</th>
                    <th class="input-column">Zone 4</th>
                    <th class="input-column"></th>
                </tr>
                <tr>
                    <td><input type="text" id="z1" name="z1"></td>
                    <td><input type="text" id="z2" name="z2"></td>
                    <td><input type="text" id="z3" name="z3"></td>
                    <td><input type="text" id="z4" name="z4"></td>
                    <td></td>
                </tr>
            </table>
        </div>
        <div style="width: 800px;  float: right;">
            <!-- Rapport TT/TC -->
            <h2 class="section-title">Rapport TT/TC</h2>
            <table>
                <tr>
                    <td><input type="text" id="rapport_tt" name="rapport_tt" placeholder="Rapport TT"></td>
                    <td><input type="text" id="rapport_tc" name="rapport_tc" placeholder="Rapport TC"></td>
                 </tr>
                 
            </table>
        </div>
    <div style="width: 800px; margin-left: 900px; margin-bottom: 50px;">
            <!-- Nouvelle section pour les entrées de Porter Resistives Phase et Terre -->
<h2 class="section-title">Porter Resistives Phase et Terre</h2>
<table>
    <tr>
        <th class="input-column">Vmin</th>
        <th class="input-column">Ith</th>
    </tr>
    <tr>
        <td><input type="text" id="vmin" name="vmin"></td>
        <td><input type="text" id="ith" name="ith"></td>
    </tr>
</table>

</div>        
<div style="width: 400px; margin-left: 1100px; margin-bottom: 50px;">
            <!-- Bouton Calculer et Effacer -->
            <div style="text-align: center;">
                <button class="calculator-button" onclick="calculer()">Calculer</button>
                <button class="calculator-button" onclick="effacerChamps()">Effacer</button>
           <!-- Résultats de la ligne protégée -->
        </div>
    </div>
    
<div id="resultat_protégée"  style="width: 800px; float: left; ">
    <h2 class="section-title">Résultats de la Ligne Protégée</h2>
    <div class="result-value" id="resultat_global_impedance"></div>
    <div class="result-value" id="resultat_global_rapport"></div>
    <div class="result-value" id="resultat_global_angle"></div>
    <div class="result-value" id="resultat_global_terre_kZ0_amplitude"></div>
    <div class="result-value" id="resultat_global_terre_kZ0_angle"></div>
</div>

<!-- Résultats des Zones -->
<div id="resultat_zone"  style="width: 800px;  float: right;">
    <h2 class="section-title">Résultats des Zones</h2>
    <div class="result-value" id="resultat_zone1_primaire"></div>
    <div class="result-value" id="resultat_zone1_secondaire"></div>
    <div class="result-value" id="resultat_zone2_primaire"></div>
    <div class="result-value" id="resultat_zone2_secondaire"></div>
    <div class="result-value" id="resultat_zone3_primaire"></div>
    <div class="result-value" id="resultat_zone3_secondaire"></div>
    <div class="result-value" id="resultat_zone4_primaire"></div>
    <div class="result-value" id="resultat_zone4_secondaire"></div>
</div>

<!-- Nouvelle section pour les entrées de Porter Resistives Phase et Terre -->
<!-- Résultats des Porter Resistives Phase et Terre -->
<!-- Résultats des Porter Resistives Phase et Terre -->
<div id="resultat_porter_resistives" class="result-container" style="width: 800px; margin-left: 900px; margin-bottom: 50px;">
    <h2 class="section-title">Résultats des Porter Resistives Phase et Terre</h2>
    <div class="result-value" id="resultat_charge_max"></div>
    <div class="result-value" id="resultat_PorteeTerre"></div>
    <div class="result-value" id="resultat_portee_phase"></div>
</div> <br><br>
<div class="result-label" id="resultat_porter_resistives" style="width: 800px; margin-left: 900px; "> Merci de bien vouloir comprendre le fonctionnement des aspects il y'a Trois sorte de resultat </div>     

</div>
</div>
</main>


<footer>
    <div class="footer-content">
        <p>Adresse: Rue Zone de Captage, Dakar</p>
        <p>Téléphone: +33 833 01 76</p>
        <p>Email: <a href="mailto:contact@example.com">contact@example.com</a></p>
    </div>
</footer>

<script src="js/html2canvas.js"></script>
<script src="js/html2pdf.js"></script>
</head>
<body>
<!-- Votre calculatrice ici -->

<!-- Votre code JavaScript -->
<script src="js/script.js"></script>

</body>
</div>

</body>
</html>
