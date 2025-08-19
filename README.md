# Shipping-Calculator<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Fatafat.fr Shipping Calculator</title>
  <style>
    body { font-family: Arial, sans-serif; max-width: 500px; margin: 40px auto; }
    label, select, input, button { display: block; margin-bottom: 15px; width: 100%; padding: 8px; }
    .result { font-weight: bold; margin-top: 20px; }
  </style>
</head>
<body>

  <h2>Shipping Calculator – Fatafat.fr</h2>

  <label for="postcode">Enter Your Postal Code:</label>
  <input type="text" id="postcode" placeholder="e.g. 75015" maxlength="5">

  <label for="orderValue">Enter Order Total (€):</label>
  <input type="number" id="orderValue" placeholder="e.g. 60">

  <button onclick="calculateShipping()">Calculate Shipping</button>

  <div class="result" id="shippingResult"></div>

  <script>
    const zones = {
      zone1: {
        codes: [75000,75001,75002,75003,75004,75005,75006,75007,75008,75009,75010,75011,75012,75013,75014,75015,75016,75017,75018,75019,75020,75116,
                92000,92100,92110,92120,92130,92150,92170,92200,92210,92230,92240,92250,92270,92300,92390,92400,92600,92700,92800,
                93000,93100,93110,93120,93130,93140,93150,93170,93200,93210,93220,93230,93240,93250,93260,93270,93300,93310,93320,93340,93350,93360,93380,93400,93420,93430,93440,93450,93500,93600,93700,93800,
                94120,94130,94160,94170,94200,94220,94250,94270,94300,94340,94410],
        freeLimit: 50,
        deliveryCharge: 6.5,
        deliveryTime: "Same day if ordered before 12h; otherwise next day",
        expressCharge: 12,
        expressAvailable: true
      },
      zone2: {
        codes: [77181,77185,77186,77200,77270,77420,77500,78000,78100,78110,78140,78150,78170,78220,78230,78290,78300,78360,78380,78400,78420,78430,78500,78560,78600,78700,78800,
                91300,91570,92140,92160,92220,92260,92310,92320,92330,92340,92350,92360,92370,92380,92410,92420,92430,92500,
                93160,93190,93290,93330,93370,93390,93410,93460,93470,
                94000,94100,94110,94140,94150,94190,94210,94230,94240,94260,94290,94310,94320,94350,94360,94380,94390,94400,94430,94450,94460,94480,94490,94500,94510,94550,94600,94700,94800,94880,
                95100,95110,95120,95130,95140,95150,95160,95170,95190,95200,95210,95230,95240,95250,95330,95350,95360,95370,95380,95400,95410,95440,95460,95480,95500,95530,95550,95580,95600,95700,95870,95880],
        deliveryCharge: 9.9,
        deliveryTime: "Same day if ordered before 12h; otherwise next day",
        expressAvailable: false
      },
      zone3: {
        codes: [], // All other codes fall here
        deliveryTime: "Within approx. 72 working hours (UPS Pickup or Home Delivery)",
        pickupFreeLimit: 55,
        pickupCharge: 9.95,
        homeDelivery: {
          over55: { 'upTo10kg': 9.95, '10To20kg': 12.95 },
          under55: { 'upTo10kg': 14.95, '10To20kg': 17.95 }
        }
      }
    };

    function calculateShipping() {
      const postcode = parseInt(document.getElementById('postcode').value);
      const orderValue = parseFloat(document.getElementById('orderValue').value);
      const result = document.getElementById('shippingResult');

      if (isNaN(postcode) || isNaN(orderValue)) {
        result.textContent = "Please enter valid postal code and order amount.";
        return;
      }

      let zoneFound = false;

      for (let [zoneName, zoneData] of Object.entries(zones)) {
        if (zoneData.codes.includes(postcode)) {
          zoneFound = true;
          if (zoneName === "zone1") {
            const charge = orderValue >= zoneData.freeLimit ? 0 : zoneData.deliveryCharge;
            result.innerHTML = `Zone: Zone 1<br>Shipping: €${charge.toFixed(2)}<br>Delivery: ${zoneData.deliveryTime}<br>Express Delivery (Paris 75): €${zoneData.expressCharge}`;
          } else if (zoneName === "zone2") {
            result.innerHTML = `Zone: Zone 2<br>Shipping: €${zoneData.deliveryCharge.toFixed(2)}<br>Delivery: ${zoneData.deliveryTime}`;
          }
          return;
        }
      }

      // Zone 3 (default)
      result.innerHTML = `Zone: Zone 3 (Rest of France)<br>
        Delivery via UPS or other partner.<br>
        Estimated Delivery: ${zones.zone3.deliveryTime}<br>
        Shipping cost depends on total weight & order value.`;
    }
  </script>

</body>
</html>
