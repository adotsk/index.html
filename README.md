<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WhatsApp Birthday Automation</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.1.3/css/bootstrap.min.css" rel="stylesheet">
    <style>
        .container {
            max-width: 800px;
        }
        .card {
            margin-bottom: 20px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .btn-primary {
            background-color: #25D366;
            border-color: #25D366;
        }
        .btn-primary:hover {
            background-color: #128C7E;
            border-color: #128C7E;
        }
        #status-message {
            display: none;
            margin-top: 20px;
        }
        .logo {
            color: #25D366;
            font-weight: bold;
        }
        .step-number {
            background-color: #25D366;
            color: white;
            border-radius: 50%;
            width: 30px;
            height: 30px;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            margin-right: 10px;
        }
    </style>
</head>
<body>
    <div class="container py-5">
        <h1 class="text-center mb-4"><span class="logo">WhatsApp</span> Birthday Automation</h1>
        
        <div class="card">
            <div class="card-header bg-light">
                <h5 class="mb-0"><span class="step-number">1</span>Connect to Google Sheets</h5>
            </div>
            <div class="card-body">
                <div class="mb-3">
                    <label for="spreadsheetId" class="form-label">Google Spreadsheet ID</label>
                    <input type="text" class="form-control" id="spreadsheetId" placeholder="Enter spreadsheet ID from the URL">
                    <div class="form-text">Find this in your Google Sheets URL: docs.google.com/spreadsheets/d/<strong>spreadsheetId</strong>/edit</div>
                </div>
                <div class="mb-3">
                    <label for="sheetName" class="form-label">Sheet Name</label>
                    <input type="text" class="form-control" id="sheetName" placeholder="Usually 'Sheet1' by default">
                </div>
                <button id="authorizeButton" class="btn btn-primary">Authorize & Connect</button>
            </div>
        </div>

        <div class="card">
            <div class="card-header bg-light">
                <h5 class="mb-0"><span class="step-number">2</span>Configure Column Mappings</h5>
            </div>
            <div class="card-body">
                <div class="row">
                    <div class="col-md-6">
                        <div class="mb-3">
                            <label for="nameColumn" class="form-label">Name Column</label>
                            <input type="text" class="form-control" id="nameColumn" placeholder="e.g., A">
                        </div>
                        <div class="mb-3">
                            <label for="phoneColumn" class="form-label">Phone Number Column</label>
                            <input type="text" class="form-control" id="phoneColumn" placeholder="e.g., B">
                            <div class="form-text">Include country code (e.g., +1234567890)</div>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="mb-3">
                            <label for="birthdayColumn" class="form-label">Birthday Column</label>
                            <input type="text" class="form-control" id="birthdayColumn" placeholder="e.g., C">
                            <div class="form-text">Format: MM/DD/YYYY</div>
                        </div>
                        <div class="mb-3">
                            <label for="salutationColumn" class="form-label">Salutation Column (Optional)</label>
                            <input type="text" class="form-control" id="salutationColumn" placeholder="e.g., D">
                        </div>
                    </div>
                </div>
                <button id="verifyColumnsButton" class="btn btn-primary">Verify Columns</button>
            </div>
        </div>

        <div class="card">
            <div class="card-header bg-light">
                <h5 class="mb-0"><span class="step-number">3</span>Customize Message Template</h5>
            </div>
            <div class="card-body">
                <div class="mb-3">
                    <label for="messageTemplate" class="form-label">Message Template</label>
                    <textarea class="form-control" id="messageTemplate" rows="4" placeholder="Write your template message here">Happy Birthday {{salutation}} {{name}}! Wishing you a fantastic day filled with joy and happiness.</textarea>
                </div>
                <div class="form-text mb-3">Use {{name}}, {{salutation}}, and other placeholders that will be replaced with data from your sheet.</div>
                <button id="testTemplateButton" class="btn btn-primary">Test Template</button>
            </div>
        </div>

        <div class="card">
            <div class="card-header bg-light">
                <h5 class="mb-0"><span class="step-number">4</span>Set Trigger Time</h5>
            </div>
            <div class="card-body">
                <div class="mb-3">
                    <label for="triggerTime" class="form-label">Time to send birthday messages</label>
                    <input type="time" class="form-control" id="triggerTime" value="09:00">
                </div>
                <div class="mb-3">
                    <label for="timezoneSelect" class="form-label">Timezone</label>
                    <select class="form-select" id="timezoneSelect">
                        <option value="UTC">UTC</option>
                        <option value="America/New_York">Eastern Time (ET)</option>
                        <option value="America/Chicago">Central Time (CT)</option>
                        <option value="America/Denver">Mountain Time (MT)</option>
                        <option value="America/Los_Angeles">Pacific Time (PT)</option>
                        <option value="Asia/Kolkata">India Standard Time (IST)</option>
                        <option value="Europe/London">Greenwich Mean Time (GMT)</option>
                        <option value="Europe/Paris">Central European Time (CET)</option>
                        <option value="Asia/Tokyo">Japan Standard Time (JST)</option>
                        <option value="Australia/Sydney">Australian Eastern Time (AET)</option>
                    </select>
                </div>
                <div class="mb-3 form-check">
                    <input type="checkbox" class="form-check-input" id="sendDayBefore">
                    <label class="form-check-label" for="sendDayBefore">Send message one day before birthday</label>
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-header bg-light">
                <h5 class="mb-0"><span class="step-number">5</span>Activate Automation</h5>
            </div>
            <div class="card-body">
                <div class="d-grid gap-2">
                    <button id="saveConfigButton" class="btn btn-primary">Save Configuration</button>
                    <button id="activateButton" class="btn btn-success">Activate Automation</button>
                </div>
                <div class="form-text mt-2">Once activated, the app will check for birthdays daily and send messages at your specified time.</div>
            </div>
        </div>

        <div class="alert alert-info" id="status-message"></div>

        <div class="card mt-4">
            <div class="card-header bg-light">
                <h5 class="mb-0">Upcoming Birthday Messages</h5>
            </div>
            <div class="card-body">
                <div class="table-responsive">
                    <table class="table table-striped" id="upcomingTable">
                        <thead>
                            <tr>
                                <th>Name</th>
                                <th>Birthday</th>
                                <th>Message Will Send On</th>
                                <th>Status</th>
                            </tr>
                        </thead>
                        <tbody>
                            <!-- Data will be inserted here by JavaScript -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

    <!-- Google API Script -->
    <script src="https://apis.google.com/js/api.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/bootstrap/5.1.3/js/bootstrap.bundle.min.js"></script>
    <script src="app.js"></script>
</body>
</html>
