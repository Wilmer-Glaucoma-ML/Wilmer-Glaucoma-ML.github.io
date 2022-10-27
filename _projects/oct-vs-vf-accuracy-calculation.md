---
layout: page
title: VF and OCT Accuracy to Detect Glaucoma Worsening
description: 
importance: 1
---
<style>
table, th, td {
  border:1px solid black;
}

label {
  display: inline;
  clear: left;
  text-align: left;
  outline: none;
}

input[type="value"] {
  outline: none !important;
  display: inline;
  width: 9ch;
  text-align: center;
  border: 2px solid black;
  border-radius: 0px;
}

</style>
<body>
  This calculator is valid for estimating accuracy over a 2-year period, but you should enter frequencies on a per-year basis. 
  <p></p>
    <div class="form-group">
      <label id="vfFreq-label" for="vfFreq"><b>Number of VFs per year &nbsp;&nbsp;</b></label>
      <input
        type="value"
        name="vfFreq"
        id="vfFreqInput"
        class="form-control"
        required
      >
    </div>
    <div class="form-group">
      <label id="measurementInVF1-label" for="measurementInVF1"> <b>Rate of VF worsening in &nbsp;&nbsp;</b></label>
      <input
        type="value"
        name="measurementInVF1"
        id="measurementInputVF1"
        class="form-control"
        required
      >
      <b>dB/year</b>
      <!-- <label id="measurementInVF2-label" for="measurementInVF2"></label>
      <input
        type="value"
        name="measurementInVF2"
        id="measurementInputVF2"
        class="form-control"
      >
      <b>percentile (50<sup>th</sup> to 99<sup>th</sup>)</b> -->
    </div>
    <hr>
    <div class="form-group">
      <label id="octFreq-label" for="octFreq"><b>Number of OCTs per year  </b></label>
      <input
        type="value"
        name="octFreq"
        id="octFreqInput"
        class="form-control"
        required
      >
    </div> 
    <div class="form-group">
      <label id="measurementInOCT1-label" for="measurementInOCT1"><b>Rate of OCT worsening in </b></label>
      <input
        type="value"
        name="measurementInOCT1"
        id="measurementInputOCT1"
        class="form-control"
        required
      >
      <b>µm/year</b>
      <!-- <label id="measurementInOCT2-label" for="measurementInOCT2"></label>
      <input
        type="value"
        name="measurementInOCT2"
        id="measurementInputOCT2"
        class="form-control"
      >
      <b>percentile (50<sup>th</sup> to 99<sup>th</sup>)</b> -->
    </div>

<div class="buttondiv">
  <button type="button" id="submit" class="submit-button" 
    onclick="getInputValue()">Calculate</button>
</div>

<p></p>
<div class="eqnAnswer"><center>
  <p id= "result"><b>Accuracies</b></p></center></div>

<table class="tb" id="accuracyTable" style="width:100%">
  <tr>
    <td style="text-align:center"><b>VF Accuracy (%)</b></td>
    <td style="text-align:center"><b>OCT Accuracy (%)</b></td>
    <td style="text-align:center"><b>Combined Accuracy (%)</b></td>
  </tr>
  <tr>
    <td style="text-align:center">&nbsp;</td>
    <td style="text-align:center">&nbsp;</td>
    <td style="text-align:center">&nbsp;</td>
  </tr>
</table>
<p><center><b>Percentiles</b></center></p>
<br>

<table class="tb" id="pctTable" style="width:100%">
  <tr>
    <td style="text-align:center"><b>VF Percentile (%)</b></td>
    <td style="text-align:center"><b>OCT Percentile (%)</b></td>
  </tr>
  <tr>
    <td style="text-align:center">&nbsp;</td>
    <td style="text-align:center">&nbsp;</td>
  </tr>
</table>

<script> 
// Function to return the index of the closest number in an array.
function closest(num, arr) {
  var curr = arr[0];
  var curr_idx = 0;
  var diff = Math.abs(num - curr);
  for (var val = 0; val < arr.length; val++) {
    var newdiff = Math.abs(num - arr[val]);
    if (newdiff < diff) {
      diff = newdiff;
      curr = arr[val];
      curr_idx = val;
    }
  }
  return curr_idx;
}

// Function to calculate the  percent correct based on our paper.
function accuracyEqn(smp, lookup_idx, lookup_type) {
  // Need to multiply the frequency value by 2 to make it on the rate of 2 years instead of the input 1 year
  smp = smp * 2;
  if (lookup_type == 'vf') {
    // Lookup table for the three vf coefficients
    const vf_lookup = {
      coef1 : [0.190479757,0.223652646,0.240565754,0.25506804,0.268316951,0.278997972,0.286546959,0.294022053,0.299776696,0.306077907,0.312791273,0.320444755,0.328436473,0.336484647,0.345284608,0.354453719,0.363793116,0.370818444,0.378462381,0.38688114,0.395880937,0.402951587,0.411322427,0.41998632,0.428177898,0.437260048,0.445671172,0.454137583,0.462847222,0.471153507,0.479536375,0.487988654,0.496396101,0.50478851,0.513602547,0.522089676,0.530245102,0.53885268,0.54736018,0.555686883,0.564053794,0.572725508,0.580857411,0.589373409,0.597666194,0.605872789,0.614307008,0.623002664,0.631558161,0.639744857],
      coef2 : [-0.06041798,-0.066359787,-0.074531655,-0.080669621,-0.084999058,-0.085814752,-0.084099306,-0.082535574,-0.079634789,-0.076995367,-0.073601837,-0.07120895,-0.068627667,-0.066425077,-0.064331883,-0.062588237,-0.06118427,-0.058473066,-0.0558686,-0.054109654,-0.053016401,-0.050437505,-0.048861819,-0.047637771,-0.045879454,-0.045139835,-0.043517384,-0.042098482,-0.04078574,-0.039490299,-0.03807006,-0.03704648,-0.036178284,-0.035116476,-0.034529478,-0.033730731,-0.032743137,-0.031969275,-0.031330375,-0.030424242,-0.029747191,-0.029353868,-0.028280956,-0.027641885,-0.026793469,-0.025912759,-0.025431125,-0.02527389,-0.024962652,-0.024181651],
      coef3 : [0.045527569,0.047352334,0.04889447,0.049296853,0.049339206,0.048587534,0.046924293,0.045291282,0.043245747,0.041354745,0.039502305,0.038044452,0.036682253,0.035357956,0.034332855,0.03340215,0.032585995,0.031353617,0.030159661,0.029229359,0.028542603,0.027370194,0.026603544,0.025993015,0.025164408,0.024632628,0.023926903,0.023263134,0.022680439,0.022084466,0.021467283,0.020900937,0.020391343,0.019910315,0.01954054,0.019102851,0.018571945,0.018185832,0.017786483,0.017378583,0.01700327,0.016717034,0.016261021,0.01590464,0.01549048,0.015100516,0.014797525,0.01458047,0.014318877,0.013954276],
    };
    var coef1 = vf_lookup['coef1'][lookup_idx];
    var coef2 = vf_lookup['coef2'][lookup_idx];
    var coef3 = vf_lookup['coef3'][lookup_idx];
  } else {
    // Lookup table for the three oct coefficients
    const oct_lookup = {
      coef1 : [0.822690159,0.793945489,0.730533405,0.589983103,0.533914205,0.491102554,0.458007414,0.431223757,0.420185995,0.414983281,0.412228807,0.412641231,0.415463034,0.419753292,0.423068375,0.428292969,0.436120729,0.442247358,0.449541672,0.457060536,0.463816713,0.469437265,0.475638675,0.482555582,0.489221053,0.495829808,0.502234568,0.509338581,0.516922551,0.524523218,0.532081511,0.539844431,0.5473206,0.555056817,0.562926995,0.571086904,0.579001527,0.585931753,0.594183503,0.601886015,0.609985808,0.617472844,0.625352327,0.633011309,0.641304675,0.649346536,0.656971262,0.664899912,0.672521023,0.680167706],
      coef2 : [-0.008314292,-0.032211032,-0.004467137,0.03043717,0.008564705,-0.020894607,-0.044184804,-0.050345584,-0.056031653,-0.06024591,-0.060154992,-0.061197386,-0.06071026,-0.059746806,-0.059179924,-0.058264602,-0.057989043,-0.056471821,-0.055329258,-0.054658946,-0.053177686,-0.051272196,-0.049749402,-0.048142539,-0.046890517,-0.04538224,-0.043532434,-0.042026527,-0.040957929,-0.039786755,-0.03855801,-0.037523299,-0.036745078,-0.035532288,-0.034532526,-0.0338042,-0.032851818,-0.031625267,-0.03123376,-0.030439212,-0.029864763,-0.028887948,-0.028216255,-0.027475399,-0.027342323,-0.02696894,-0.026231624,-0.025883057,-0.025363683,-0.024688808],
      coef3 : [0.012032007,0.018014356,0.012613292,0.011711992,0.022474625,0.0331204,0.039687026,0.040200388,0.040566082,0.040634876,0.03973449,0.039018301,0.038152903,0.037290654,0.036253457,0.03527301,0.034704043,0.033741618,0.03300032,0.032375419,0.031521536,0.030433141,0.029466941,0.028545242,0.027795006,0.02692258,0.025972371,0.025116939,0.024501165,0.023903164,0.023258957,0.022651519,0.022122248,0.02155156,0.020999924,0.02055514,0.020047982,0.019420669,0.019078847,0.018591768,0.018230707,0.017714526,0.01734582,0.016909745,0.016646404,0.016356228,0.015924305,0.015619082,0.015273325,0.014877935],
    };
    var coef1 = oct_lookup['coef1'][lookup_idx];
    var coef2 = oct_lookup['coef2'][lookup_idx];
    var coef3 = oct_lookup['coef3'][lookup_idx];
  }
  return 100*(coef1 + coef2*Math.log(smp) + coef3*(Math.log(smp))**2);
}

// Calculate combined accuracy of vf and oct.
function combinedAccuracy(vf_percent_correct, oct_percent_correct) {
  vf_cd = vf_percent_correct / 100;
  oct_vd = oct_percent_correct / 100;
  return 100*(vf_cd + oct_vd - (vf_cd*oct_vd));
}

// Function that handles the main information flow to perform the calculations.
function calculateAccuracy(vf_freq, oct_freq, vf_measurement_input, oct_measurement_input) {
  // VF Tables
  const vf_percentiles = [0.99,0.98,0.97,0.96,0.95,0.94,0.93,0.92,0.91,0.9,0.89,0.88,0.87,0.86,0.85,0.84,0.83,0.82,0.81,0.8,0.79,0.78,0.77,0.76,0.75,0.74,0.73,0.72,0.71,0.7,0.69,0.68,0.67,0.66,0.65,0.64,0.63,0.62,0.61,0.6,0.59,0.58,0.57,0.56,0.55,0.54,0.53,0.52,0.51,0.5];
  const db_pyear = [-2.375374179,-1.87072991,-1.55882768,-1.354906634,-1.209968329,-1.10005846,-1.004460165,-0.922879731,-0.850669103,-0.788616302,-0.73812498,-0.694618166,-0.657602418,-0.622323225,-0.594619155,-0.569041382,-0.545408203,-0.51918586,-0.495643593,-0.474408781,-0.455362716,-0.43417603,-0.41702764,-0.401546448,-0.38535631,-0.370813706,-0.357395279,-0.344351564,-0.332899923,-0.32056724,-0.30908592,-0.296439662,-0.284030788,-0.273758069,-0.263631487,-0.253395759,-0.242297973,-0.2336445,-0.223724021,-0.215218543,-0.206087714,-0.197988734,-0.189993294,-0.182100379,-0.174189643,-0.166953802,-0.159186689,-0.151801921,-0.144212655,-0.137260077];
  // OCT Tables
  const oct_percentiles = [0.99,0.98,0.97,0.96,0.95,0.94,0.93,0.92,0.91,0.9,0.89,0.88,0.87,0.86,0.85,0.84,0.83,0.82,0.81,0.8,0.79,0.78,0.77,0.76,0.75,0.74,0.73,0.72,0.71,0.7,0.69,0.68,0.67,0.66,0.65,0.64,0.63,0.62,0.61,0.6,0.59,0.58,0.57,0.56,0.55,0.54,0.53,0.52,0.51,0.5];
  const mic_pyear = [-59.10676741,-21.06141589,-10.61985367,-6.43719542,-4.867978254,-3.929476474,-3.29888707,-2.868740323,-2.575112527,-2.348728534,-2.172631297,-2.020366034,-1.899064351,-1.797026962,-1.68954472,-1.602117914,-1.533984613,-1.464555097,-1.407078688,-1.350655596,-1.296282342,-1.237350255,-1.181807662,-1.134068909,-1.088528277,-1.04379893,-1.000612736,-0.961338114,-0.92650516,-0.894411349,-0.862917964,-0.831970156,-0.798927213,-0.772183197,-0.7445282,-0.718996287,-0.69435203,-0.664350749,-0.639762671,-0.61411954,-0.591691858,-0.566843864,-0.546191311,-0.522478076,-0.500657912,-0.48070557,-0.457669937,-0.436351304,-0.415084497,-0.39408508];
  var vf_change = 0;
  var oct_change = 0;
  // Calculate VF information
  var vf_lookup_idx = closest(vf_measurement_input, db_pyear);
  vf_change = vf_percentiles[vf_lookup_idx] * 100;
  // Calculate OCT information
  var oct_lookup_idx = closest(oct_measurement_input, mic_pyear);
  oct_change = oct_percentiles[oct_lookup_idx] * 100;
  
  // Use the accuracy equations to calculate percent correct for everything
  var vf_percent_correct = accuracyEqn(vf_freq, vf_lookup_idx, 'vf');
  var oct_percent_correct = accuracyEqn(oct_freq, oct_lookup_idx, 'oct');
  var combined_percent_correct = combinedAccuracy(vf_percent_correct, oct_percent_correct);
  return [vf_percent_correct, vf_change, oct_percent_correct, oct_change, combined_percent_correct];
}

// Get number of significant figures from inputs - MUST be string
function getSignificantDigitCount(n) {
  var dec_sent = false;
  // If input has a decimal place, set flag to true and append "1" to the end of it.
  // This is a hack around JavaScript engine automatically converting "1.00" to "1"
  // but now it will accurately keep it as "1.001" and then we subtract 1 sig fig at the end if flag is true.
  if (n.indexOf(".") != -1) {
    n += 1
    dec_sent = true;
  }
  n = Math.abs(n.replace(".", "")); //remove decimal and make positive
  if (n == 0) return 0;
  while (n != 0 && n % 10 == 0) n /= 10; //kill the 0s at the end of n
  if (dec_sent) {
    return Math.floor(Math.log(n) / Math.LN10)
  }
  return Math.floor(Math.log(n) / Math.LN10) + 1; //get number of digits
}

// This is where all the values from the page are actually read in and then the table values are updated. 
function getInputValue() {
  // Read in all inputs
  var vf_freq = Math.round(document.getElementById("vfFreqInput").value);
  var vf_rate = document.getElementById("measurementInputVF1").value;
  var oct_freq = Math.round(document.getElementById("octFreqInput").value);
  var oct_rate = document.getElementById("measurementInputOCT1").value;
  // Flags to check if only VF or only OCT info has been input
  var only_vf = false;
  var only_oct = false;
  // First check if VF is the only thing that has been input
  if (!oct_freq && !oct_rate) {
    only_vf = true;
    var min_sigs = getSignificantDigitCount(vf_rate);
  // Then check if OCT is the only thing that has been input
  } else if (!vf_freq && !vf_rate) {
    only_oct = true;
    var min_sigs = getSignificantDigitCount(oct_rate);
  } else {
    // Get significant figures for rounding later (ignoring frequency inputs since they are integers)
    vf_rate_sigs = getSignificantDigitCount(vf_rate);
    oct_rate_sigs = getSignificantDigitCount(oct_rate);
    var min_sigs = Math.min(vf_rate_sigs, oct_rate_sigs);
  }

  // Calculate accuracies
  var pct_array = calculateAccuracy(vf_freq, oct_freq, vf_rate, oct_rate);
  var vf_percent_correct = pct_array[0];
  var vf_change = pct_array[1];
  var oct_percent_correct = pct_array[2];
  var oct_change = pct_array[3];
  var combined_percent_correct = pct_array[4];
  //Round decimal places of accuracies to minimum sig figs from above
  vf_percent_correct = vf_percent_correct.toPrecision(min_sigs);
  oct_percent_correct = oct_percent_correct.toPrecision(min_sigs);
  combined_percent_correct = combined_percent_correct.toPrecision(min_sigs);
  // Update tables
  var acc_table = document.getElementById('accuracyTable');
  var pct_table = document.getElementById('pctTable');
  if (only_vf) {
    acc_table.rows[1].cells[0].innerHTML = vf_percent_correct;
    acc_table.rows[1].cells[1].innerHTML = "";
    acc_table.rows[1].cells[2].innerHTML = "";
    pct_table.rows[1].cells[0].innerHTML = vf_change;
    pct_table.rows[1].cells[1].innerHTML = "";
  } else if (only_oct) {
    acc_table.rows[1].cells[0].innerHTML = "";
    acc_table.rows[1].cells[1].innerHTML = oct_percent_correct;
    acc_table.rows[1].cells[2].innerHTML = "";
    pct_table.rows[1].cells[0].innerHTML = "";
    pct_table.rows[1].cells[1].innerHTML = oct_change.toPrecision(2);
  } else {
    acc_table.rows[1].cells[0].innerHTML = vf_percent_correct;
    acc_table.rows[1].cells[1].innerHTML = oct_percent_correct;
    acc_table.rows[1].cells[2].innerHTML = combined_percent_correct;
    pct_table.rows[1].cells[0].innerHTML = vf_change;
    pct_table.rows[1].cells[1].innerHTML = oct_change.toPrecision(2);
  }
  
} 
</script>
</body>