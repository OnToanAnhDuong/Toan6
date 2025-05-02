
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QUẢN LÝ THỜI KHÓA BIỂU</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.1/xlsx.full.min.js"></script>
    <style>
     body {
    font-family: Arial, sans-serif;
    background-color: #f5f6fa;
    margin: 0;
    padding: 0;
}

.container {
    max-width: 1000px;
    margin: 30px auto;
    background: white;
    padding: 30px;
    border-radius: 12px;
    box-shadow: 0 0 12px rgba(0, 0, 0, 0.1);
}

h1 {
    text-align: center;
    color: hsl(215, 92%, 51%);
    margin-bottom: 30px;
}

.upload-section, .filter-section, .lookup-section {
    margin-bottom: 25px;
    padding: 15px;
    border: 1px solid #dcdde1;
    border-radius: 8px;
    background-color: hwb(236 4% 4%);
}

label {
    display: inline-block;
    margin-right: 20px;
    font-weight: bold;
    color: lab(100% 0.01 -0.01);
}

select, input[type="file"] {
    padding: 5px;
    margin: 5px 10px 5px 0;
    font-size: 14px;
}

button {
    padding: 6px 14px;
    font-size: 14px;
    background-color: #00a8ff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #8e00e6;
}

#output, #lookupResult, #lookupResultByClass {
    font-size: 18px;
    color: #ed6b07;
    margin-top: 10px;
}

#output ul {
    list-style-type: none;
    padding-left: 20px;
    counter-reset: list-item;
    margin: 5px 0;
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
    gap: 35px;
}

#output li {
    padding-left: 10px;
    position: relative;
    counter-increment: list-item;
    margin-bottom: 5px;
    font-size: 16px;
    color: #2d3436;
    font-weight: bold;
    background-color: #f5f6fa;
    padding: 8px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
    transition: background-color 0.3s ease;
    display: flex;
    align-items: center;
}

#output li:hover {
    background-color: #dfe6e9;
}

#output li::before {
    content: counter(list-item) ". ";
    position: absolute;
    left: -30px;
    font-weight: bold;
    color: #00a8ff;
    font-size: 18px;
    margin-right: 10px;
}

.table-container {
    width: 100%;
    max-width: 100%;
    overflow-x: auto; /* Thêm thanh cuộn ngang */
    overflow-y: auto; /* Thêm thanh cuộn dọc */
    margin-top: 20px;
    max-height: 500px; /* Giới hạn chiều cao của bảng */
}

table {
    width: 100%;
    border-collapse: collapse;
}

th, td {
    padding: 10px;
    text-align: center;
    border: 1px solid #dcdde1;
    box-sizing: border-box; /* Đảm bảo không có dư thừa padding hoặc border */
}

/* Cố định hàng tiêu đề khi cuộn dọc */
thead th {
    position: sticky;
    top: 0;
    background-color: #00a8ff;
    z-index: 2;
}

/* Cố định cột 1 (Ngày) khi trượt ngang */
th:first-child, td:first-child {
    position: sticky;
    left: 0;
    background-color: #f5f6fa; /* Màu nền cho cột cố định */
    z-index: 2; /* Đảm bảo cột này ở trên các cột khác */
}

/* Cố định cột 2 (Tiết) khi trượt ngang */
th:nth-child(2), td:nth-child(2) {
    position: sticky;
    left: 120px; /* Điều chỉnh vị trí của cột 2 ngay sau cột 1 */
    background-color: #f5f6fa;
    z-index: 3; /* Cột 2 sẽ ở trên nhưng thấp hơn cột 1 */
}

tbody {
    background-color: #f5f6fa;
}

    </style>
</head>
<body>
<div class="container">
    <h1>QUẢN LÝ THỜI KHÓA BIỂU</h1>

    <!-- Upload file -->
    <div class="upload-section">
        <label for="fileInput">Chọn File Thời Khóa Biểu:</label>
        <input type="file" id="fileInput" accept=".xlsx" onchange="handleFile()" />
    </div>

    <!-- Lọc giáo viên trống tiết -->
    <div class="filter-section">
        <label for="daySelect">Chọn ngày:</label>
        <select id="daySelect"></select>

        <label for="periodSelect">Chọn tiết:</label>
        <select id="periodSelect"></select>

        <button id="filterButton" onclick="filterTeachers()">Lọc Giáo viên Trống Tiết</button>
    </div>

    <div id="output">
        <p>Danh sách giáo viên trống tiết sẽ hiển thị ở đây.</p>
    </div>

    <!-- Tra cứu theo lớp -->
    <div class="lookup-section">
        <h3>Tra cứu theo Lớp - Tiết - Thứ</h3>
        <label for="daySelect2">Chọn Thứ:</label>
        <select id="daySelect2"></select>

        <label for="periodSelect2">Chọn Tiết:</label>
        <select id="periodSelect2"></select>

        <label for="classSelect">Chọn Lớp:</label>
        <select id="classSelect"></select>

        <button onclick="lookupByClassPeriod()">Tra cứu giáo viên</button>
    </div>
    <!-- Khung hiển thị cho tra cứu theo lớp và tiết -->
    <div id="lookupResultByClass"></div>
    <!-- Tra cứu theo giáo viên -->
    <div class="lookup-section">
        <h3>Tra cứu theo Giáo viên - Tiết - Thứ</h3>
        <label for="daySelect3">Chọn Thứ:</label>
        <select id="daySelect3"></select>

        <label for="periodSelect3">Chọn Tiết:</label>
        <select id="periodSelect3"></select>

        <label for="teacherSelect">Chọn Giáo viên:</label>
        <select id="teacherSelect"></select>

        <button onclick="lookupByTeacher()">Tra cứu lớp và môn</button>
    </div>

    <div id="lookupResult"></div>

    <!-- Bảng thời khóa biểu -->
    <div class="table-container">
        <table id="timetable" class="table">
            <thead></thead>
            <tbody></tbody>
        </table>
    </div>
</div>

<script>
function handleFile() {
    const fileInput = document.getElementById('fileInput');
    const file = fileInput.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
            const data = event.target.result;
            const workbook = XLSX.read(data, { type: 'binary' });
            const sheet = workbook.Sheets[workbook.SheetNames[0]];
            const jsonData = XLSX.utils.sheet_to_json(sheet, { header: 1 });
            createTimetable(jsonData);
            populateDropdowns(jsonData);
            populateAdditionalDropdowns(jsonData);
        };
        reader.readAsBinaryString(file);
    }
}

function createTimetable(data) {
    const tableBody = document.getElementById('timetable').querySelector('tbody');
    const tableHead = document.getElementById('timetable').querySelector('thead');
    tableBody.innerHTML = '';
    tableHead.innerHTML = '';

    const headerRow = document.createElement('tr');
    data[0].forEach(cell => {
        const th = document.createElement('th');
        th.textContent = cell;
        headerRow.appendChild(th);
    });
    tableHead.appendChild(headerRow);

    data.slice(1).forEach(row => {
        const tr = document.createElement('tr');
        row.forEach(cell => {
            const td = document.createElement('td');
            td.textContent = cell;
            tr.appendChild(td);
        });
        tableBody.appendChild(tr);
    });
}

function populateDropdowns(data) {
    const daySelect = document.getElementById('daySelect');
    const periodSelect = document.getElementById('periodSelect');
    const days = [...new Set(data.slice(1).map(row => row[0]))];
    const periods = [...new Set(data.slice(1).map(row => row[1]))];
    days.forEach(day => {
        const opt = document.createElement('option');
        opt.value = day;
        opt.textContent = day;
        daySelect.appendChild(opt);
    });
    periods.forEach(period => {
        const opt = document.createElement('option');
        opt.value = period;
        opt.textContent = period;
        periodSelect.appendChild(opt);
    });
}

function populateAdditionalDropdowns(data) {
    const classes = data[0].slice(2);
    const classSelect = document.getElementById('classSelect');
    classes.forEach(cls => {
        const opt = document.createElement('option');
        opt.value = cls;
        opt.textContent = cls;
        classSelect.appendChild(opt);
    });

    const days = [...new Set(data.slice(1).map(row => row[0]))];
    const periods = [...new Set(data.slice(1).map(row => row[1]))];
    [ 'daySelect2', 'daySelect3' ].forEach(id => {
        const sel = document.getElementById(id);
        days.forEach(day => {
            const opt = document.createElement('option');
            opt.value = day;
            opt.textContent = day;
            sel.appendChild(opt);
        });
    });
    [ 'periodSelect2', 'periodSelect3' ].forEach(id => {
        const sel = document.getElementById(id);
        periods.forEach(period => {
            const opt = document.createElement('option');
            opt.value = period;
            opt.textContent = period;
            sel.appendChild(opt);
        });
    });

    const teachers = getTeacherCodes();
    const teacherSelect = document.getElementById('teacherSelect');
    teachers.forEach(teacher => {
        const opt = document.createElement('option');
        opt.value = teacher;
        opt.textContent = teacher;
        teacherSelect.appendChild(opt);
    });
}

function getTeacherCodes() {
    const rows = document.getElementById('timetable').rows;
    const codes = new Set();
    for (let i = 1; i < rows.length; i++) {
        const cells = rows[i].cells;
        for (let j = 2; j < cells.length; j++) {
            const parts = cells[j].textContent.split('-');
            if (parts[1]) codes.add(parts[1].trim());
        }
    }
    return Array.from(codes);
}

function filterTeachers() {
    const day = document.getElementById('daySelect').value;
    const period = document.getElementById('periodSelect').value;
    const rows = document.getElementById('timetable').rows;
    const teachers = getTeacherCodes();
    let targetRow = -1;
    for (let i = 1; i < rows.length; i++) {
        const cells = rows[i].cells;
        if (cells[0].textContent === day && cells[1].textContent === period) {
            targetRow = i;
            break;
        }
    }
    if (targetRow === -1) return;
    const busy = new Set();
    const cells = rows[targetRow].cells;
    for (let j = 2; j < cells.length; j++) {
        const parts = cells[j].textContent.split('-');
        if (parts[1]) busy.add(parts[1].trim());
    }
    const free = teachers.filter(t => !busy.has(t));
    const output = document.getElementById('output');
    output.innerHTML = free.length ? `<strong>DANH SACH GIÁO VIÊN TRỐNG TIẾT:</strong><ul>${free.map(t => `<li>${t}</li>`).join('')}</ul>` : 'Không có giáo viên trống.';
}

function lookupByClassPeriod() {
    const day = document.getElementById('daySelect2').value;
    const period = document.getElementById('periodSelect2').value;
    const className = document.getElementById('classSelect').value;
    const table = document.getElementById('timetable');
    const rows = table.rows;
    const headers = rows[0].cells;
    let colIndex = -1;
    for (let i = 2; i < headers.length; i++) {
        if (headers[i].textContent === className) {
            colIndex = i; break;
        }
    }
    if (colIndex === -1) return;
    for (let i = 1; i < rows.length; i++) {
        const cells = rows[i].cells;
        if (cells[0].textContent === day && cells[1].textContent === period) {
            const info = cells[colIndex].textContent;
            document.getElementById('lookupResultByClass').innerHTML = `Tiết ${period} lớp ${className}: <strong>${info}</strong>`;
            return;
        }
    }
    document.getElementById('lookupResultByClass').innerHTML = 'Không tìm thấy.';
}

function lookupByTeacher() {
    const day = document.getElementById('daySelect3').value;
    const period = document.getElementById('periodSelect3').value;
    const teacher = document.getElementById('teacherSelect').value;
    const table = document.getElementById('timetable');
    const rows = table.rows;
    const headers = rows[0].cells;
    for (let i = 1; i < rows.length; i++) {
        const cells = rows[i].cells;
        if (cells[0].textContent === day && cells[1].textContent === period) {
            for (let j = 2; j < cells.length; j++) {
                if (cells[j].textContent.includes(teacher)) {
                    const info = cells[j].textContent;
                    const cls = headers[j].textContent;
                    document.getElementById('lookupResult').innerHTML = `GV ${teacher} dạy <strong>${info}</strong> lớp <strong>${cls}</strong>`;
                    return;
                }
            }
        }
    }
    document.getElementById('lookupResult').innerHTML = 'Giáo viên này trống tiết.';
}
</script>
</body>
</html>
