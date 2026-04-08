# **webpage code**



<!DOCTYPE html>

<html lang="en">

<head>

  <meta charset="UTF-8">

  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <title>Water Management System</title>

  <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/3.9.1/chart.min.js"></script>

  <style>

    \* { margin: 0; padding: 0; box-sizing: border-box; }

    body {

      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;

      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

      min-height: 100vh;

      padding: 20px;

    }

    .hidden { display: none !important; }

    .container { max-width: 1400px; margin: 0 auto; }

 

    /\* Login \*/

    .login-box {

      background: white;

      padding: 40px;

      border-radius: 15px;

      box-shadow: 0 20px 60px rgba(0,0,0,0.3);

      max-width: 400px;

      margin: 100px auto;

    }

    .login-box h1 { text-align: center; color: #667eea; margin-bottom: 30px; }

    .form-group { margin-bottom: 20px; }

    .form-group label { display: block; margin-bottom: 8px; font-weight: 600; }

    .form-group input {

      width: 100%;

      padding: 12px;

      border: 2px solid #e0e0e0;

      border-radius: 8px;

      font-size: 16px;

    }

    .btn {

      width: 100%;

      padding: 14px;

      background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);

      color: white;

      border: none;

      border-radius: 8px;

      font-size: 16px;

      font-weight: 600;

      cursor: pointer;

      transition: opacity 0.3s;

    }

    .btn:hover { opacity: 0.9; }

    .error { color: #dc3545; text-align: center; margin-top: 15px; }

 

    /\* Dashboard \*/

    .header {

      background: white;

      padding: 20px;

      border-radius: 15px;

      margin-bottom: 20px;

      display: flex;

      justify-content: space-between;

      align-items: center;

      flex-wrap: wrap;

    }

    .header h1 { color: #667eea; }

    .btn-logout {

      padding: 10px 20px;

      background: #6c757d;

      color: white;

      border: none;

      border-radius: 8px;

      cursor: pointer;

      font-weight: 600;

    }

    .grid {

      display: grid;

      grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));

      gap: 20px;

      margin-bottom: 20px;

    }

    .card {

      background: white;

      padding: 25px;

      border-radius: 15px;

      box-shadow: 0 4px 15px rgba(0,0,0,0.1);

    }

    .card h2 { margin-bottom: 20px; color: #333; }

    .flow-value {

      font-size: 48px;

      font-weight: bold;

      color: #667eea;

      text-align: center;

      margin: 20px 0;

    }

    .flow-unit { text-align: center; color: #666; }

    .sensor-status {

      display: inline-block;

      width: 12px;

      height: 12px;

      border-radius: 50%;

      margin-left: 10px;

      animation: pulse 2s infinite;

    }

    @keyframes pulse {

      0%, 100% { opacity: 1; }

      50% { opacity: 0.5; }

    }

    .sensor-status.online { background: #28a745; box-shadow: 0 0 10px #28a745; }

    .sensor-status.offline { background: #dc3545; box-shadow: 0 0 10px #dc3545; animation: none; }

    .sensor-status.error { background: #ff6b6b; box-shadow: 0 0 10px #ff6b6b; }

    .info-row {

      display: flex;

      justify-content: space-between;

      padding: 10px 0;

      border-bottom: 1px solid #e0e0e0;

    }

    .info-row:last-child { border-bottom: none; }

    .alert {

      padding: 15px;

      border-radius: 8px;

      margin-bottom: 20px;

      font-weight: 600;

      animation: slideIn 0.3s ease-out;

    }

    @keyframes slideIn {

      from { transform: translateY(-10px); opacity: 0; }

      to { transform: translateY(0); opacity: 1; }

    }

    .alert-danger { background: #f8d7da; color: #721c24; border-left: 4px solid #dc3545; }

    .alert-warning { background: #fff3cd; color: #856404; border-left: 4px solid #ffc107; }

    .alert-success { background: #d4edda; color: #155724; border-left: 4px solid #28a745; }

    .alert-info { background: #d1ecf1; color: #0c5460; border-left: 4px solid #17a2b8; }

 

    /\* Switch \*/

    .switch {

      position: relative;

      display: inline-block;

      width: 60px;

      height: 34px;

    }

    .switch input { opacity: 0; width: 0; height: 0; }

    .slider {

      position: absolute;

      cursor: pointer;

      top: 0; left: 0; right: 0; bottom: 0;

      background-color: #ccc;

      transition: .4s;

      border-radius: 34px;

    }

    .slider:before {

      position: absolute;

      content: "";

      height: 26px;

      width: 26px;

      left: 4px;

      bottom: 4px;

      background-color: white;

      transition: .4s;

      border-radius: 50%;

    }

    input:checked + .slider { background-color: #28a745; }

    input:checked + .slider:before { transform: translateX(26px); }

 

    /\* Table \*/

    table { width: 100%; border-collapse: collapse; margin-top: 20px; }

    th, td { padding: 12px; text-align: left; border-bottom: 1px solid #e0e0e0; }

    th { background: #f8f9fa; font-weight: 600; }

    .badge {

      display: inline-block;

      padding: 4px 12px;

      border-radius: 12px;

      font-size: 12px;

      font-weight: 600;

    }

    .badge-success { background: #d4edda; color: #155724; }

    .badge-warning { background: #fff3cd; color: #856404; }

    .btn-small {

      padding: 6px 12px;

      font-size: 14px;

      border-radius: 6px;

      border: none;

      cursor: pointer;

      background: #667eea;

      color: white;

      font-weight: 600;

    }

    .btn-small:hover { opacity: 0.8; }



    /\* Payment Modal \*/

    .modal {

      position: fixed;

      top: 0;

      left: 0;

      width: 100%;

      height: 100%;

      background: rgba(0,0,0,0.7);

      display: flex;

      justify-content: center;

      align-items: center;

      z-index: 1000;

    }

    .modal-content {

      background: white;

      padding: 30px;

      border-radius: 15px;

      max-width: 500px;

      width: 90%;

      box-shadow: 0 10px 40px rgba(0,0,0,0.3);

      animation: modalSlide 0.3s ease-out;

    }

    @keyframes modalSlide {

      from { transform: translateY(-50px); opacity: 0; }

      to { transform: translateY(0); opacity: 1; }

    }

    .modal-header {

      display: flex;

      justify-content: space-between;

      align-items: center;

      margin-bottom: 20px;

      padding-bottom: 15px;

      border-bottom: 2px solid #e0e0e0;

    }

    .modal-header h2 { color: #667eea; margin: 0; }

    .close-btn {

      background: none;

      border: none;

      font-size: 28px;

      cursor: pointer;

      color: #999;

      line-height: 1;

    }

    .close-btn:hover { color: #333; }

    .payment-option {

      padding: 15px;

      border: 2px solid #e0e0e0;

      border-radius: 10px;

      margin-bottom: 15px;

      cursor: pointer;

      transition: all 0.3s;

      display: flex;

      align-items: center;

    }

    .payment-option:hover { border-color: #667eea; background: #f8f9ff; }

    .payment-option.selected { border-color: #667eea; background: #f0f3ff; }

    .payment-option input\[type="radio"] { margin-right: 15px; width: 20px; height: 20px; }

    .payment-details { margin-top: 20px; }

    .payment-details input {

      width: 100%;

      padding: 12px;

      margin-bottom: 15px;

      border: 2px solid #e0e0e0;

      border-radius: 8px;

      font-size: 16px;

    }

    .btn-pay {

      width: 100%;

      padding: 15px;

      background: linear-gradient(135deg, #28a745 0%, #20c997 100%);

      color: white;

      border: none;

      border-radius: 8px;

      font-size: 18px;

      font-weight: 600;

      cursor: pointer;

      transition: opacity 0.3s;

    }

    .btn-pay:hover { opacity: 0.9; }

    .payment-summary {

      background: #f8f9fa;

      padding: 15px;

      border-radius: 8px;

      margin-bottom: 20px;

    }

    .payment-summary .row {

      display: flex;

      justify-content: space-between;

      margin-bottom: 10px;

    }

    .payment-summary .total {

      font-size: 20px;

      font-weight: bold;

      color: #667eea;

      padding-top: 10px;

      border-top: 2px solid #dee2e6;

      margin-top: 10px;

    }



    /\* Status indicators \*/

    .status-indicator {

      display: inline-flex;

      align-items: center;

      padding: 8px 15px;

      border-radius: 20px;

      font-weight: 600;

      font-size: 14px;

    }

    .status-normal { background: #d4edda; color: #155724; }

    .status-leak { background: #fff3cd; color: #856404; }

    .status-theft { background: #f8d7da; color: #721c24; }

    .status-error { background: #f8d7da; color: #721c24; }

  </style>

</head>

<body>

 

  <!-- Login Page -->

  <div id="loginPage">

    <div class="login-box">

      <h1>💧 Water Management</h1>

      <div class="form-group">

        <label>Username</label>

        <input type="text" id="username" value="panchayat">

      </div>

      <div class="form-group">

        <label>Password</label>

        <input type="password" id="password" value="admin123">

      </div>

      <button class="btn" onclick="login()">Login</button>

      <div id="loginError" class="error"></div>

      <div style="margin-top: 20px; padding-top: 20px; border-top: 1px solid #e0e0e0; font-size: 12px; color: #666;">

        <p><strong>Demo Credentials:</strong></p>

        <p>Admin: panchayat / admin123</p>

        <p>House1: house1 / house1pass</p>

        <p>House2: house2 / house2pass</p>

      </div>

    </div>

  </div>



  <!-- Admin Dashboard -->

  <div id="adminDash" class="hidden">

    <div class="container">

      <div class="header">

        <h1>🏛️ Panchayat Control Panel</h1>

        <button class="btn-logout" onclick="logout()">Logout</button>

      </div>



      <div id="alertContainer"></div>



      <div class="grid">

        <div class="card">

          <h2>🌊 Main Supply <span class="sensor-status online" id="mainStatus"></span></h2>

          <div class="flow-value" id="mainFlow">0.00</div>

          <div class="flow-unit">L/min</div>

          <div class="info-row">

            <span>Pulses:</span>

            <span id="mainPulse">0</span>

          </div>

          <div class="info-row">

            <span>Status:</span>

            <span id="mainSensorStatus">Operational</span>

          </div>

          <div class="info-row">

            <span>Updated:</span>

            <span id="mainTime">-</span>

          </div>

        </div>



        <div class="card">

          <h2>🏠 House 1 <span class="sensor-status online" id="h1Status"></span></h2>

          <div class="flow-value" id="h1Flow">0.00</div>

          <div class="flow-unit">L/min</div>

          <div class="info-row">

            <span>Pulses:</span>

            <span id="h1Pulse">0</span>

          </div>

          <div class="info-row">

            <span>Status:</span>

            <span id="h1SensorStatus">Operational</span>

          </div>

          <div class="info-row">

            <span>Updated:</span>

            <span id="h1Time">-</span>

          </div>

        </div>



        <div class="card">

          <h2>🏠 House 2 <span class="sensor-status online" id="h2Status"></span></h2>

          <div class="flow-value" id="h2Flow">0.00</div>

          <div class="flow-unit">L/min</div>

          <div class="info-row">

            <span>Pulses:</span>

            <span id="h2Pulse">0</span>

          </div>

          <div class="info-row">

            <span>Status:</span>

            <span id="h2SensorStatus">Operational</span>

          </div>

          <div class="info-row">

            <span>Updated:</span>

            <span id="h2Time">-</span>

          </div>

        </div>

      </div>



      <!-- Detailed Flow Analysis Section -->

      <div class="card" style="margin-bottom: 20px;">

        <h2>🔍 Flow Analysis \& Detection System</h2>

 

        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 20px; margin-bottom: 25px;">

          <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border-left: 4px solid #667eea;">

            <div style="font-size: 12px; color: #666; font-weight: 600; margin-bottom: 5px;">MAIN SUPPLY</div>

            <div style="font-size: 28px; font-weight: bold; color: #667eea;" id="detailMainFlow">0.00</div>

            <div style="font-size: 14px; color: #666;">L/min</div>

          </div>

 

          <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border-left: 4px solid #28a745;">

            <div style="font-size: 12px; color: #666; font-weight: 600; margin-bottom: 5px;">HOUSE 1 FLOW</div>

            <div style="font-size: 28px; font-weight: bold; color: #28a745;" id="detailH1Flow">0.00</div>

            <div style="font-size: 14px; color: #666;">L/min</div>

          </div>

 

          <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border-left: 4px solid #ffc107;">

            <div style="font-size: 12px; color: #666; font-weight: 600; margin-bottom: 5px;">HOUSE 2 FLOW</div>

            <div style="font-size: 28px; font-weight: bold; color: #ffc107;" id="detailH2Flow">0.00</div>

            <div style="font-size: 14px; color: #666;">L/min</div>

          </div>

 

          <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; border-left: 4px solid #17a2b8;">

            <div style="font-size: 12px; color: #666; font-weight: 600; margin-bottom: 5px;">TOTAL CONSUMPTION</div>

            <div style="font-size: 28px; font-weight: bold; color: #17a2b8;" id="detailTotalFlow">0.00</div>

            <div style="font-size: 14px; color: #666;">L/min</div>

          </div>

        </div>



        <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 25px; border-radius: 12px; color: white; margin-bottom: 25px;">

          <div style="display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px;">

            <div>

              <div style="font-size: 14px; opacity: 0.9; margin-bottom: 5px;">FLOW DIFFERENCE</div>

              <div style="font-size: 36px; font-weight: bold;" id="detailDifference">0.00</div>

              <div style="font-size: 14px; opacity: 0.9;">L/min</div>

            </div>

            <div style="text-align: right;">

              <div style="font-size: 14px; opacity: 0.9; margin-bottom: 5px;">CALCULATION</div>

              <div style="font-size: 16px; font-weight: 600;" id="detailFormula">Main - (H1 + H2)</div>

              <div style="font-size: 14px; opacity: 0.9; margin-top: 5px;" id="detailCalcExample">0.00 - (0.00 + 0.00) = 0.00</div>

            </div>

          </div>

        </div>



        <div style="background: #fff; border: 2px solid #e0e0e0; border-radius: 12px; padding: 20px;">

          <h3 style="margin-bottom: 15px; color: #333;">🎯 Detection Thresholds \& Status</h3>

 

          <div style="display: grid; gap: 15px;">

            <div style="display: flex; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 8px;">

              <div style="width: 40px; height: 40px; border-radius: 50%; background: #28a745; display: flex; align-items: center; justify-content: center; margin-right: 15px;">

                <span style="color: white; font-size: 20px;">✓</span>

              </div>

              <div style="flex: 1;">

                <div style="font-weight: 600; color: #28a745; margin-bottom: 3px;">NORMAL OPERATION</div>

                <div style="font-size: 14px; color: #666;">Difference ≤ 0.5 L/min - System working properly</div>

              </div>

              <div id="statusNormal" style="padding: 5px 15px; border-radius: 20px; background: #d4edda; color: #155724; font-weight: 600; font-size: 12px;">INACTIVE</div>

            </div>



            <div style="display: flex; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 8px;">

              <div style="width: 40px; height: 40px; border-radius: 50%; background: #ffc107; display: flex; align-items: center; justify-content: center; margin-right: 15px;">

                <span style="color: white; font-size: 20px;">⚠</span>

              </div>

              <div style="flex: 1;">

                <div style="font-weight: 600; color: #ffc107; margin-bottom: 3px;">WATER LEAKAGE</div>

                <div style="font-size: 14px; color: #666;">Difference: 0.5 - 1.0 L/min - Minor leak in pipeline</div>

              </div>

              <div id="statusLeak" style="padding: 5px 15px; border-radius: 20px; background: #fff3cd; color: #856404; font-weight: 600; font-size: 12px;">INACTIVE</div>

            </div>



            <div style="display: flex; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 8px;">

              <div style="width: 40px; height: 40px; border-radius: 50%; background: #dc3545; display: flex; align-items: center; justify-content: center; margin-right: 15px;">

                <span style="color: white; font-size: 20px;">🚨</span>

              </div>

              <div style="flex: 1;">

                <div style="font-weight: 600; color: #dc3545; margin-bottom: 3px;">WATER THEFT</div>

                <div style="font-size: 14px; color: #666;">Difference > 1.0 L/min - Unauthorized water diversion</div>

              </div>

              <div id="statusTheft" style="padding: 5px 15px; border-radius: 20px; background: #f8d7da; color: #721c24; font-weight: 600; font-size: 12px;">INACTIVE</div>

            </div>



            <div style="display: flex; align-items: center; padding: 15px; background: #f8f9fa; border-radius: 8px;">

              <div style="width: 40px; height: 40px; border-radius: 50%; background: #6c757d; display: flex; align-items: center; justify-content: center; margin-right: 15px;">

                <span style="color: white; font-size: 20px;">✕</span>

              </div>

              <div style="flex: 1;">

                <div style="font-weight: 600; color: #6c757d; margin-bottom: 3px;">SENSOR ERROR</div>

                <div style="font-size: 14px; color: #666;">Sensor malfunction detected - Readings unreliable</div>

              </div>

              <div id="statusSensor" style="padding: 5px 15px; border-radius: 20px; background: #e2e3e5; color: #383d41; font-weight: 600; font-size: 12px;">INACTIVE</div>

            </div>

          </div>

        </div>



        <div style="margin-top: 20px; padding: 15px; background: #e7f3ff; border-left: 4px solid #2196F3; border-radius: 8px;">

          <div style="font-weight: 600; color: #1976D2; margin-bottom: 5px;">ℹ️ How Detection Works:</div>

          <div style="font-size: 14px; color: #555; line-height: 1.6;">

            The system continuously monitors the main supply flow and compares it with the sum of individual house consumptions.

            Any significant difference indicates either water leakage in the distribution network or unauthorized water theft through illegal connections.

          </div>

        </div>

      </div>



      <div class="grid">

        <div class="card">

          <h2>⚙️ Controls</h2>

          <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px;">

            <span style="font-weight: 600;">Solenoid Valve</span>

            <label class="switch">

              <input type="checkbox" id="solenoid" checked onchange="toggleSolenoid()">

              <span class="slider"></span>

            </label>

          </div>

          <label style="font-weight: 600; display: block; margin-bottom: 10px;">Calibration Factor</label>

          <input type="number" id="calibration" value="7.5" step="0.1" style="width: 100%; padding: 10px; margin-bottom: 10px; border: 2px solid #e0e0e0; border-radius: 8px;">

          <button class="btn" onclick="updateCal()">Update Calibration</button>

        </div>



        <div class="card">

          <h2>📊 System Analysis</h2>

          <div class="info-row">

            <span>Main Flow:</span>

            <span id="analysisMain">0.00 L/min</span>

          </div>

          <div class="info-row">

            <span>Houses Total:</span>

            <span id="analysisHouses">0.00 L/min</span>

          </div>

          <div class="info-row">

            <span>Difference:</span>

            <span id="analysisDiff">0.00 L/min</span>

          </div>

          <div class="info-row">

            <span>System Status:</span>

            <span id="analysisStatus" class="status-indicator status-normal">Normal</span>

          </div>

        </div>



        <div class="card">

          <h2>📈 Flow Chart</h2>

          <canvas id="chart"></canvas>

        </div>

      </div>



      <div class="card">

        <h2>💰 Bills Management</h2>

        <button class="btn" style="width: auto; margin-bottom: 20px;" onclick="showBillForm()">Generate New Bill</button>

        <div id="billForm" class="hidden" style="background: #f8f9fa; padding: 20px; border-radius: 8px; margin-bottom: 20px;">

          <input type="text" id="month" placeholder="Month (e.g., Jan)" style="padding: 10px; margin: 5px; border: 2px solid #ddd; border-radius: 5px;">

          <input type="number" id="year" placeholder="Year" style="padding: 10px; margin: 5px; border: 2px solid #ddd; border-radius: 5px;">

          <input type="number" id="h1usage" placeholder="House 1 Usage (L)" style="padding: 10px; margin: 5px; border: 2px solid #ddd; border-radius: 5px;">

          <input type="number" id="h2usage" placeholder="House 2 Usage (L)" style="padding: 10px; margin: 5px; border: 2px solid #ddd; border-radius: 5px;">

          <input type="number" id="rate" placeholder="Rate per L (₹)" step="0.01" value="0.05" style="padding: 10px; margin: 5px; border: 2px solid #ddd; border-radius: 5px;">

          <button class="btn" style="width: auto; margin: 5px;" onclick="genBill()">Generate Bills</button>

        </div>

        <table id="billTable">

          <thead><tr><th>House</th><th>Month</th><th>Usage (L)</th><th>Amount</th><th>Status</th></tr></thead>

          <tbody></tbody>

        </table>

      </div>

    </div>

  </div>



  <!-- User Dashboard -->

  <div id="userDash" class="hidden">

    <div class="container">

      <div class="header">

        <h1>🏠 <span id="houseName"></span></h1>

        <button class="btn-logout" onclick="logout()">Logout</button>

      </div>



      <div id="userAlertContainer"></div>



      <div class="grid">

        <div class="card">

          <h2>💧 Current Flow <span class="sensor-status online" id="userStatus"></span></h2>

          <div class="flow-value" id="userFlow">0.00</div>

          <div class="flow-unit">L/min</div>

          <div class="info-row">

            <span>Sensor Status:</span>

            <span id="userSensorStatus">Online</span>

          </div>

          <div class="info-row">

            <span>Last Updated:</span>

            <span id="userTime">-</span>

          </div>

        </div>



        <div class="card">

          <h2>📊 This Month</h2>

          <div class="info-row">

            <span>Usage:</span>

            <span id="monthUsage">0 L</span>

          </div>

          <div class="info-row">

            <span>Estimated Bill:</span>

            <span id="monthBill">₹0</span>

          </div>

          <div class="info-row">

            <span>Payment Status:</span>

            <span id="payStatus">-</span>

          </div>

        </div>

      </div>



      <div class="card">

        <h2>💰 Payment History</h2>

        <table id="userBillTable">

          <thead><tr><th>Month</th><th>Usage (L)</th><th>Amount</th><th>Status</th><th>Action</th></tr></thead>

          <tbody></tbody>

        </table>

      </div>

    </div>

  </div>



  <!-- Payment Modal -->

  <div id="paymentModal" class="modal hidden">

    <div class="modal-content">

      <div class="modal-header">

        <h2>💳 Payment Gateway</h2>

        <button class="close-btn" onclick="closePayment()">\&times;</button>

      </div>

 

      <div class="payment-summary">

        <div class="row">

          <span>Bill Month:</span>

          <span id="payMonth">-</span>

        </div>

        <div class="row">

          <span>Water Usage:</span>

          <span id="payUsage">-</span>

        </div>

        <div class="row total">

          <span>Total Amount:</span>

          <span id="payAmount">₹0</span>

        </div>

      </div>



      <h3 style="margin-bottom: 15px; color: #333;">Select Payment Method</h3>

 

      <div class="payment-option" onclick="selectPayment('upi')">

        <input type="radio" name="payment" id="upiRadio" value="upi">

        <label for="upiRadio" style="cursor: pointer; flex: 1;">

          <strong>UPI</strong><br>

          <small style="color: #666;">Pay using UPI ID or QR code</small>

        </label>

      </div>



      <div class="payment-option" onclick="selectPayment('card')">

        <input type="radio" name="payment" id="cardRadio" value="card">

        <label for="cardRadio" style="cursor: pointer; flex: 1;">

          <strong>Credit/Debit Card</strong><br>

          <small style="color: #666;">Visa, Mastercard, Rupay</small>

        </label>

      </div>



      <div class="payment-option" onclick="selectPayment('netbanking')">

        <input type="radio" name="payment" id="netbankingRadio" value="netbanking">

        <label for="netbankingRadio" style="cursor: pointer; flex: 1;">

          <strong>Net Banking</strong><br>

          <small style="color: #666;">All major banks supported</small>

        </label>

      </div>



      <div id="paymentDetails" class="payment-details hidden">

        <!-- UPI Details -->

        <div id="upiDetails" class="hidden">

          <input type="text" id="upiId" placeholder="Enter UPI ID (e.g., user@paytm)">

        </div>



        <!-- Card Details -->

        <div id="cardDetails" class="hidden">

          <input type="text" id="cardNumber" placeholder="Card Number" maxlength="16">

          <input type="text" id="cardName" placeholder="Cardholder Name">

          <div style="display: flex; gap: 10px;">

            <input type="text" id="cardExpiry" placeholder="MM/YY" maxlength="5" style="flex: 1;">

            <input type="text" id="cardCvv" placeholder="CVV" maxlength="3" style="flex: 1;">

          </div>

        </div>



        <!-- Net Banking Details -->

        <div id="netbankingDetails" class="hidden">

          <select id="bankSelect" style="width: 100%; padding: 12px; margin-bottom: 15px; border: 2px solid #e0e0e0; border-radius: 8px; font-size: 16px;">

            <option value="">Select Your Bank</option>

            <option value="sbi">State Bank of India</option>

            <option value="hdfc">HDFC Bank</option>

            <option value="icici">ICICI Bank</option>

            <option value="axis">Axis Bank</option>

            <option value="pnb">Punjab National Bank</option>

            <option value="other">Other</option>

          </select>

        </div>



        <button class="btn-pay" onclick="processPayment()">Pay Now</button>

      </div>

    </div>

  </div>



  <script>

    const users = {

      'panchayat': { pass: 'admin123', role: 'admin' },

      'house1': { pass: 'house1pass', role: 'user', house: 'H1', name: 'House 1' },

      'house2': { pass: 'house2pass', role: 'user', house: 'H2', name: 'House 2' }

    };



    let user = null;

    let data = {

      main: {f:0, p:0, t:Date.now(), working:true},

      h1: {f:0, p:0, t:Date.now(), working:true},

      h2: {f:0, p:0, t:Date.now(), working:true}

    };

    let cal = 7.5;

    let bills = \[

      {id:1, h:'H1', n:'House 1', m:'Nov', y:2024, u:5000, a:250, p:true},

      {id:2, h:'H2', n:'House 2', m:'Nov', y:2024, u:4500, a:225, p:false},

      {id:3, h:'H1', n:'House 1', m:'Dec', y:2024, u:5200, a:260, p:false},

      {id:4, h:'H2', n:'House 2', m:'Dec', y:2024, u:4800, a:240, p:false}

    ];

    let chart;

    let currentBillId = null;

    let selectedPaymentMethod = null;

 

    // Simulation parameters

    let sensorFailureChance = 0.02; // 2% chance per cycle

    let theftSimulation = false;

    let leakSimulation = false;



   function login() {

  const u = document.getElementById('username').value;

  const p = document.getElementById('password').value;

 

  if (users\[u] \&\& users\[u].pass === p) {

    user = {username: u, ...users\[u]};

    document.getElementById('loginPage').classList.add('hidden');

 

    if (user.role === 'admin') {

      document.getElementById('adminDash').classList.remove('hidden');

      initChart();

      // startSim();

    } else {

      document.getElementById('userDash').classList.remove('hidden');

      document.getElementById('houseName').textContent = user.name;

      // startSim();

    }



    loadBills();



    // ✅ ADD THIS LINE HERE (IMPORTANT)

    setInterval(fetchESPData, 1000);



  } else {

    document.getElementById('loginError').textContent = 'Invalid credentials';

  }

}

function fetchESPData() {

  fetch("http://192.168.4.1/data")   // ESP32 AP default IP

    .then(res => res.json())

    .then(json => {

      console.log("ESP DATA:", json); // DEBUG (important)



      // Map ESP data to your existing structure

      data.main = {

        f: json.main.f,

        p: json.main.p,

        t: Date.now(),

        working: json.main.working

      };



      data.h1 = {

        f: json.h1.f,

        p: json.h1.p,

        t: Date.now(),

        working: json.h1.working

      };



      data.h2 = {

        f: json.h2.f,

        p: json.h2.p,

        t: Date.now(),

        working: json.h2.working

      };



      update(); // 🔥 THIS updates the UI

    })

    .catch(err => {

      console.error("ESP fetch error:", err);

    });

}







    function logout() {

      location.reload();

    }



    function initChart() {

      const ctx = document.getElementById('chart').getContext('2d');

      chart = new Chart(ctx, {

        type: 'line',

        data: {

          labels: \[],

          datasets: \[

            {label: 'Main Supply', data: \[], borderColor: '#667eea', backgroundColor: 'rgba(102, 126, 234, 0.1)', tension: 0.4, fill: true},

            {label: 'House 1', data: \[], borderColor: '#28a745', backgroundColor: 'rgba(40, 167, 69, 0.1)', tension: 0.4, fill: true},

            {label: 'House 2', data: \[], borderColor: '#ffc107', backgroundColor: 'rgba(255, 193, 7, 0.1)', tension: 0.4, fill: true}

          ]

        },

        options: {

          responsive: true,

          maintainAspectRatio: true,

          scales: {

            y: {

              beginAtZero: true,

              title: { display: true, text: 'Flow Rate (L/min)' }

            },

            x: {

              title: { display: true, text: 'Time' }

            }

          },

          plugins: {

            legend: { display: true, position: 'top' }

          }

        }

      });

    }



   /\* function startSim() {

     // setInterval(() => {

        // Simulate sensor failures randomly

       // if (Math.random() < sensorFailureChance) {

         // const sensors = \['main', 'h1', 'h2'];

          //const failingSensor = sensors\[Math.floor(Math.random() \* sensors.length)];

          //data\[failingSensor].working = false;

          //setTimeout(() => { data\[failingSensor].working = true; }, 5000); // Recover after 5 seconds

        //}



        // Generate flow values

        const h1 = data.h1.working ? Math.random() \* 3 + 1 : 0;

        const h2 = data.h2.working ? Math.random() \* 3 + 1 : 0;

 

        let main;

        if (data.main.working) {

          main = h1 + h2 + (Math.random() \* 0.4 - 0.2); // Normal operation with small variance

 

          // Simulate theft (water being diverted) - random 5% chance

          if (Math.random() < 0.05) {

            main = h1 + h2 + (Math.random() \* 1.5 + 1); // Extra flow indicating theft

          }

 

          // Simulate leak - random 3% chance

          if (Math.random() < 0.03) {

            main = h1 + h2 + (Math.random() \* 0.8 + 0.3); // Small constant leak

          }

        } else {

          main = 0;

        }

 

        data.main = {f: parseFloat(main.toFixed(2)), p: Math.floor(main\*cal), t: Date.now(), working: data.main.working};

        data.h1 = {f: parseFloat(h1.toFixed(2)), p: Math.floor(h1\*cal), t: Date.now(), working: data.h1.working};

        data.h2 = {f: parseFloat(h2.toFixed(2)), p: Math.floor(h2\*cal), t: Date.now(), working: data.h2.working};

 

        update();

      }, 1000);

    }\*/



    function detectIssues() {

      const issues = \[];

      const mainFlow = data.main.f;

      const housesTotal = data.h1.f + data.h2.f;

      const difference = Math.abs(mainFlow - housesTotal);

      const threshold = 0.5; // Tolerance threshold in L/min

 

      // Check sensor health

      if (!data.main.working) {

        issues.push({type: 'error', msg: '🔴 Main supply sensor is CORRUPTED or not responding!'});

      }

      if (!data.h1.working) {

        issues.push({type: 'error', msg: '🔴 House 1 sensor is CORRUPTED or not responding!'});

      }

      if (!data.h2.working) {

        issues.push({type: 'error', msg: '🔴 House 2 sensor is CORRUPTED or not responding!'});

      }

 

      // Only check for theft/leak if sensors are working and there's actual flow

      if (data.main.working \&\& data.h1.working \&\& data.h2.working \&\& mainFlow > 0.3) {

        if (mainFlow > housesTotal + threshold) {

          const excess = mainFlow - housesTotal;

          if (excess > 1.0) {

            issues.push({type: 'danger', msg: `🚨 WATER THEFT DETECTED! Excess flow: ${excess.toFixed(2)} L/min`});

          } else {

            issues.push({type: 'warning', msg: `⚠️ WATER LEAK DETECTED! Leakage: ${excess.toFixed(2)} L/min`});

          }

        }

      }

 

      return issues;

    }



    function update() {

      const issues = detectIssues();

 

      if (user.role === 'admin') {

        // Update flow displays

        document.getElementById('mainFlow').textContent = data.main.f.toFixed(2);

        document.getElementById('mainPulse').textContent = data.main.p;

        document.getElementById('mainTime').textContent = new Date(data.main.t).toLocaleTimeString();

        document.getElementById('mainStatus').className = `sensor-status ${data.main.working ? 'online' : 'error'}`;

        document.getElementById('mainSensorStatus').textContent = data.main.working ? '✅ Operational' : '❌ Corrupted';

 

        document.getElementById('h1Flow').textContent = data.h1.f.toFixed(2);

        document.getElementById('h1Pulse').textContent = data.h1.p;

        document.getElementById('h1Time').textContent = new Date(data.h1.t).toLocaleTimeString();

        document.getElementById('h1Status').className = `sensor-status ${data.h1.working ? 'online' : 'error'}`;

        document.getElementById('h1SensorStatus').textContent = data.h1.working ? '✅ Operational' : '❌ Corrupted';

 

        document.getElementById('h2Flow').textContent = data.h2.f.toFixed(2);

        document.getElementById('h2Pulse').textContent = data.h2.p;

        document.getElementById('h2Time').textContent = new Date(data.h2.t).toLocaleTimeString();

        document.getElementById('h2Status').className = `sensor-status ${data.h2.working ? 'online' : 'error'}`;

        document.getElementById('h2SensorStatus').textContent = data.h2.working ? '✅ Operational' : '❌ Corrupted';

 

        // Update analysis

        const housesTotal = data.h1.f + data.h2.f;

        const diff = data.main.f - housesTotal; // Signed difference

        const absDiff = Math.abs(diff);

 

        document.getElementById('analysisMain').textContent = data.main.f.toFixed(2) + ' L/min';

        document.getElementById('analysisHouses').textContent = housesTotal.toFixed(2) + ' L/min';

        document.getElementById('analysisDiff').textContent = absDiff.toFixed(2) + ' L/min';

 

        // Update detailed analysis section

        document.getElementById('detailMainFlow').textContent = data.main.f.toFixed(2);

        document.getElementById('detailH1Flow').textContent = data.h1.f.toFixed(2);

        document.getElementById('detailH2Flow').textContent = data.h2.f.toFixed(2);

        document.getElementById('detailTotalFlow').textContent = housesTotal.toFixed(2);

        document.getElementById('detailDifference').textContent = diff.toFixed(2);

        document.getElementById('detailCalcExample').textContent = `${data.main.f.toFixed(2)} - (${data.h1.f.toFixed(2)} + ${data.h2.f.toFixed(2)}) = ${diff.toFixed(2)}`;

 

        // Update detection status badges

        const hasError = issues.some(i => i.type === 'error');

        const hasTheft = issues.some(i => i.type === 'danger');

        const hasLeak = issues.some(i => i.type === 'warning');

        const isNormal = !hasError \&\& !hasTheft \&\& !hasLeak;

 

        // Reset all status badges

        document.getElementById('statusNormal').textContent = 'INACTIVE';

        document.getElementById('statusNormal').style.background = '#e2e3e5';

        document.getElementById('statusNormal').style.color = '#6c757d';

 

        document.getElementById('statusLeak').textContent = 'INACTIVE';

        document.getElementById('statusLeak').style.background = '#e2e3e5';

        document.getElementById('statusLeak').style.color = '#6c757d';

 

        document.getElementById('statusTheft').textContent = 'INACTIVE';

        document.getElementById('statusTheft').style.background = '#e2e3e5';

        document.getElementById('statusTheft').style.color = '#6c757d';

 

        document.getElementById('statusSensor').textContent = 'INACTIVE';

        document.getElementById('statusSensor').style.background = '#e2e3e5';

        document.getElementById('statusSensor').style.color = '#6c757d';

 

        // Activate relevant status

        if (hasError) {

          document.getElementById('statusSensor').textContent = '🔴 ACTIVE';

          document.getElementById('statusSensor').style.background = '#f8d7da';

          document.getElementById('statusSensor').style.color = '#721c24';

          document.getElementById('statusSensor').style.fontWeight = 'bold';

          document.getElementById('statusSensor').style.animation = 'pulse 1s infinite';

        }

 

        if (hasTheft) {

          document.getElementById('statusTheft').textContent = '🚨 ACTIVE';

          document.getElementById('statusTheft').style.background = '#f8d7da';

          document.getElementById('statusTheft').style.color = '#721c24';

          document.getElementById('statusTheft').style.fontWeight = 'bold';

          document.getElementById('statusTheft').style.animation = 'pulse 1s infinite';

        }

 

        if (hasLeak) {

          document.getElementById('statusLeak').textContent = '⚠️ ACTIVE';

          document.getElementById('statusLeak').style.background = '#fff3cd';

          document.getElementById('statusLeak').style.color = '#856404';

          document.getElementById('statusLeak').style.fontWeight = 'bold';

          document.getElementById('statusLeak').style.animation = 'pulse 1s infinite';

        }

 

        if (isNormal) {

          document.getElementById('statusNormal').textContent = '✅ ACTIVE';

          document.getElementById('statusNormal').style.background = '#d4edda';

          document.getElementById('statusNormal').style.color = '#155724';

          document.getElementById('statusNormal').style.fontWeight = 'bold';

        }

 

        // Update status indicator

        const statusEl = document.getElementById('analysisStatus');

        if (issues.length === 0) {

          statusEl.textContent = '✅ Normal';

          statusEl.className = 'status-indicator status-normal';

        } else if (issues.some(i => i.type === 'danger')) {

          statusEl.textContent = '🚨 Theft Detected';

          statusEl.className = 'status-indicator status-theft';

        } else if (issues.some(i => i.type === 'warning')) {

          statusEl.textContent = '⚠️ Leak Detected';

          statusEl.className = 'status-indicator status-leak';

        } else if (issues.some(i => i.type === 'error')) {

          statusEl.textContent = '❌ Sensor Error';

          statusEl.className = 'status-indicator status-error';

        }

 

        // Display alerts

        const alertContainer = document.getElementById('alertContainer');

        if (issues.length > 0) {

          alertContainer.innerHTML = issues.map(i =>

            `<div class="alert alert-${i.type}">${i.msg}</div>`

          ).join('');

        } else {

          alertContainer.innerHTML = '<div class="alert alert-success">✅ All systems operating normally</div>';

        }

 

        updateChart();

      } else {

        // User dashboard

        const house = data\[user.house.toLowerCase()];

        document.getElementById('userFlow').textContent = house.f.toFixed(2);

        document.getElementById('userTime').textContent = new Date(house.t).toLocaleTimeString();

        document.getElementById('userStatus').className = `sensor-status ${house.working ? 'online' : 'offline'}`;

        document.getElementById('userSensorStatus').textContent = house.working ? '✅ Online' : '❌ Offline';

 

        // Show sensor alert for users

        const userAlertContainer = document.getElementById('userAlertContainer');

        if (!house.working) {

          userAlertContainer.innerHTML = '<div class="alert alert-warning">⚠️ Your water meter sensor is experiencing issues. Please contact Panchayat office.</div>';

        } else {

          userAlertContainer.innerHTML = '';

        }

      }

    }



    function updateChart() {

      if (!chart) return;

      const time = new Date().toLocaleTimeString();

      chart.data.labels.push(time);

      chart.data.datasets\[0].data.push(data.main.f);

      chart.data.datasets\[1].data.push(data.h1.f);

      chart.data.datasets\[2].data.push(data.h2.f);

      if (chart.data.labels.length > 20) {

        chart.data.labels.shift();

        chart.data.datasets.forEach(d => d.data.shift());

      }

      chart.update('none');

    }



    function toggleSolenoid() {

      const state = document.getElementById('solenoid').checked;

      alert('Solenoid valve ' + (state ? 'OPENED ✅' : 'CLOSED ❌'));

    }



    function updateCal() {

      cal = parseFloat(document.getElementById('calibration').value);

      alert('Calibration factor updated to ' + cal + ' pulses/L');

    }



    function loadBills() {

      if (user.role === 'admin') {

        const tbody = document.querySelector('#billTable tbody');

        tbody.innerHTML = bills.map(b => `

          <tr>

            <td>${b.n}</td>

            <td>${b.m} ${b.y}</td>

            <td>${b.u.toLocaleString()}</td>

            <td>₹${b.a.toFixed(2)}</td>

            <td><span class="badge ${b.p?'badge-success':'badge-warning'}">${b.p?'✅ Paid':'⏳ Pending'}</span></td>

          </tr>

        `).join('');

      } else {

        const ub = bills.filter(b => b.h === user.house);

        const tbody = document.querySelector('#userBillTable tbody');

        tbody.innerHTML = ub.map(b => `

          <tr>

            <td>${b.m} ${b.y}</td>

            <td>${b.u.toLocaleString()}</td>

            <td>₹${b.a.toFixed(2)}</td>

            <td><span class="badge ${b.p?'badge-success':'badge-warning'}">${b.p?'✅ Paid':'⏳ Pending'}</span></td>

            <td>${!b.p ? `<button class="btn-small" onclick="openPayment(${b.id})">Pay Now</button>` : '✅ Paid'}</td>

          </tr>

        `).join('');

 

        const cur = ub.find(b => b.m === 'Dec' \&\& b.y === 2024);

        if (cur) {

          document.getElementById('monthUsage').textContent = cur.u.toLocaleString() + ' L';

          document.getElementById('monthBill').textContent = '₹' + cur.a.toFixed(2);

          document.getElementById('payStatus').innerHTML = cur.p ? '<span class="badge badge-success">✅ Paid</span>' : '<span class="badge badge-warning">⏳ Pending</span>';

        }

      }

    }



    function openPayment(id) {

      currentBillId = id;

      const bill = bills.find(b => b.id === id);

      if (bill) {

        document.getElementById('payMonth').textContent = `${bill.m} ${bill.y}`;

        document.getElementById('payUsage').textContent = `${bill.u.toLocaleString()} L`;

        document.getElementById('payAmount').textContent = `₹${bill.a.toFixed(2)}`;

        document.getElementById('paymentModal').classList.remove('hidden');

      }

    }



    function closePayment() {

      document.getElementById('paymentModal').classList.add('hidden');

      selectedPaymentMethod = null;

      document.getElementById('paymentDetails').classList.add('hidden');

      document.querySelectorAll('.payment-option').forEach(el => el.classList.remove('selected'));

      document.querySelectorAll('input\[name="payment"]').forEach(el => el.checked = false);

    }



    function selectPayment(method) {

      selectedPaymentMethod = method;

 

      // Update UI

      document.querySelectorAll('.payment-option').forEach(el => el.classList.remove('selected'));

      document.getElementById(method + 'Radio').checked = true;

      document.getElementById(method + 'Radio').parentElement.parentElement.classList.add('selected');

 

      // Show payment details

      document.getElementById('paymentDetails').classList.remove('hidden');

      document.getElementById('upiDetails').classList.add('hidden');

      document.getElementById('cardDetails').classList.add('hidden');

      document.getElementById('netbankingDetails').classList.add('hidden');

 

      if (method === 'upi') {

        document.getElementById('upiDetails').classList.remove('hidden');

      } else if (method === 'card') {

        document.getElementById('cardDetails').classList.remove('hidden');

      } else if (method === 'netbanking') {

        document.getElementById('netbankingDetails').classList.remove('hidden');

      }

    }



    function processPayment() {

      if (!selectedPaymentMethod) {

        alert('Please select a payment method');

        return;

      }

 

      let isValid = false;

 

      if (selectedPaymentMethod === 'upi') {

        const upiId = document.getElementById('upiId').value;

        if (upiId \&\& upiId.includes('@')) {

          isValid = true;

        } else {

          alert('Please enter a valid UPI ID');

          return;

        }

      } else if (selectedPaymentMethod === 'card') {

        const cardNumber = document.getElementById('cardNumber').value;

        const cardName = document.getElementById('cardName').value;

        const cardExpiry = document.getElementById('cardExpiry').value;

        const cardCvv = document.getElementById('cardCvv').value;

 

        if (cardNumber \&\& cardName \&\& cardExpiry \&\& cardCvv \&\& cardNumber.length >= 15) {

          isValid = true;

        } else {

          alert('Please fill all card details correctly');

          return;

        }

      } else if (selectedPaymentMethod === 'netbanking') {

        const bank = document.getElementById('bankSelect').value;

        if (bank) {

          isValid = true;

        } else {

          alert('Please select your bank');

          return;

        }

      }

 

      if (isValid) {

        // Simulate payment processing

        const bill = bills.find(b => b.id === currentBillId);

        if (bill) {

          setTimeout(() => {

            bill.p = true;

            alert(`✅ Payment Successful!\\\\n\\\\nAmount: ₹${bill.a.toFixed(2)}\\\\nMethod: ${selectedPaymentMethod.toUpperCase()}\\\\nTransaction ID: TXN${Date.now()}\\\\n\\\\nThank you for your payment!`);

            closePayment();

            loadBills();

          }, 1500);

        }

      }

    }



    function showBillForm() {

      document.getElementById('billForm').classList.toggle('hidden');

    }



    function genBill() {

      const m = document.getElementById('month').value;

      const y = parseInt(document.getElementById('year').value);

      const h1u = parseInt(document.getElementById('h1usage').value);

      const h2u = parseInt(document.getElementById('h2usage').value);

      const r = parseFloat(document.getElementById('rate').value);

 

      if (!m || !y || !h1u || !h2u || !r) {

        alert('Please fill all fields');

        return;

      }

 

      bills.push(

        {id:bills.length+1, h:'H1', n:'House 1', m, y, u:h1u, a:h1u\*r, p:false},

        {id:bills.length+2, h:'H2', n:'House 2', m, y, u:h2u, a:h2u\*r, p:false}

      );

 

      alert('✅ Bills generated successfully for ' + m + ' ' + y);

      showBillForm();

      loadBills();

 

      // Clear form

      document.getElementById('month').value = '';

      document.getElementById('year').value = '';

      document.getElementById('h1usage').value = '';

      document.getElementById('h2usage').value = '';

    }

function fetchESPData() {

  fetch('/data')

    .then(r => r.json())

    .then(j => {



      data.main.f = j.main.flow;

      data.main.p = j.main.pulses;

      data.main.t = Date.now();

      data.main.working = j.main.working;



      data.h1.f = j.h1.flow;

      data.h1.p = j.h1.pulses;

      data.h1.t = Date.now();

      data.h1.working = j.h1.working;



      data.h2.f = j.h2.flow;

      data.h2.p = j.h2.pulses;

      data.h2.t = Date.now();

      data.h2.working = j.h2.working;



      update(); // reuse your existing UI logic

    })

    .catch(() => {

      console.warn('ESP32 not reachable');

    });

}



  </script>

</body>

</html>

