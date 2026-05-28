<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Saudi Aramco Team Task Tracker 🏭</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@300;400;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@latest/tabler-icons.min.css">
    
    <style>
        :root {
            --aramco-green: #00853f;
            --aramco-blue: #003366;
            --bg-color: #f4f6f9;
            --card-bg: #ffffff;
            --text-color: #333333;
            --border-color: #e0e0e0;
            --pending: #ffc107;
            --in-progress: #17a2b8;
            --done: #28a745;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Cairo', sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            padding: 20px;
        }

        header {
            background: linear-gradient(135deg, var(--aramco-blue), var(--aramco-green));
            color: white;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
        }

        .header-title h1 {
            font-size: 24px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .header-title p {
            font-size: 14px;
            opacity: 0.9;
        }

        .admin-btn {
            background-color: rgba(255, 255, 255, 0.2);
            color: white;
            border: 1px solid white;
            padding: 8px 16px;
            border-radius: 5px;
            cursor: pointer;
            transition: 0.3s;
            display: flex;
            align-items: center;
            gap: 8px;
            font-weight: 600;
        }

        .admin-btn:hover {
            background-color: white;
            color: var(--aramco-blue);
        }

        .admin-btn.logged-in {
            background-color: #dc3545;
            border-color: #dc3545;
            color: white;
        }

        /* قسم الإحصائيات */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }

        .stat-card {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 8px;
            border-right: 5px solid var(--aramco-blue);
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .stat-card.pending { border-right-color: var(--pending); }
        .stat-card.progress { border-right-color: var(--in-progress); }
        .stat-card.done { border-right-color: var(--done); }

        /* الفلاتر */
        .filters {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: flex;
            gap: 15px;
            align-items: center;
            flex-wrap: wrap;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        .filters select {
            padding: 8px 12px;
            border-radius: 5px;
            border: 1px solid var(--border-color);
            outline: none;
            font-weight: 600;
        }

        /* المحتوى الرئيسي */
        .main-content {
            display: grid;
            grid-template-columns: 280px 1fr;
            gap: 20px;
        }

        @media (max-width: 768px) {
            .main-content { grid-template-columns: 1fr; }
        }

        /* القائمة الجانبية للموظفين */
        .team-panel {
            background: var(--card-bg);
            padding: 15px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        .panel-title {
            font-size: 18px;
            margin-bottom: 15px;
            padding-bottom: 8px;
            border-bottom: 2px solid var(--bg-color);
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .member-item {
            display: flex;
            align-items: center;
            gap: 12px;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
            transition: 0.2s;
            margin-bottom: 5px;
        }

        .member-item:hover, .member-item.active {
            background-color: var(--bg-color);
        }

        .member-avatar {
            width: 40px;
            height: 40px;
            background: var(--aramco-blue);
            color: white;
            border-radius: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .member-info h4 { font-size: 14px; }
        .member-info p { font-size: 12px; color: #777; }

        /* جدول المهام */
        .tasks-panel {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
        }

        .tasks-table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 15px;
            text-align: right;
        }

        .tasks-table th, .tasks-table td {
            padding: 12px;
            border-bottom: 1px solid var(--border-color);
        }

        .tasks-table th {
            background-color: var(--bg-color);
            color: var(--aramco-blue);
        }

        .badge {
            padding: 4px 8px;
            border-radius: 4px;
            font-size: 12px;
            font-weight: 600;
            color: white;
            display: inline-block;
        }

        .badge.pm { background-color: #6f42c1; }
        .badge.cm { background-color: #fd7e14; }

        .status-dot {
            display: inline-flex;
            align-items: center;
            gap: 6px;
            font-size: 13px;
        }
        .status-dot::before {
            content: '';
            width: 10px;
            height: 10px;
            border-radius: 50%;
        }
        .status-pending::before { background-color: var(--pending); }
        .status-progress::before { background-color: var(--in-progress); }
        .status-done::before { background-color: var(--done); }

        .close-task-btn {
            background-color: #dc3545;
            color: white;
            border: none;
            padding: 5px 10px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 12px;
            display: none;
        }

        .admin-mode .close-task-btn {
            display: inline-block;
        }

        footer {
            margin-top: 30px;
            text-align: center;
            font-size: 12px;
            color: #777;
            border-top: 1px solid var(--border-color);
            padding-top: 15px;
        }
    </style>
</head>
<body>

    <header>
        <div class="header-title">
            <h1><i class="ti ti-building-factory-2"></i> Saudi Aramco Team Task Tracker</h1>
            <p>لوحة تحكم إدارة وتابعة مهام المشاريع لفرق أرامكو السعودية</p>
        </div>
        <button class="admin-btn" id="adminBtn" onclick="toggleAdmin()">
            <i class="ti ti-lock"></i> <span id="adminBtnText">دخول المدير</span>
        </button>
    </header>

    <div class="stats-grid">
        <div class="stat-card">
            <div><h5>إجمالي المهام</h5><h2 id="totalTasks">0</h2></div>
            <i class="ti ti-list-details style-icon" style="font-size: 32px; color: var(--aramco-blue);"></i>
        </div>
        <div class="stat-card pending">
            <div><h5>معلقة (Pending)</h5><h2 id="pendingTasks">0</h2></div>
            <i class="ti ti-clock-pause" style="font-size: 32px; color: var(--pending);"></i>
        </div>
        <div class="stat-card progress">
            <div><h5>جارية (In Progress)</h5><h2 id="progressTasks">0</h2></div>
            <i class="ti ti-player-play" style="font-size: 32px; color: var(--in-progress);"></i>
        </div>
        <div class="stat-card done">
            <div><h5>مكتملة (Done)</h5><h2 id="doneTasks">0</h2></div>
            <i class="ti ti-circle-check" style="font-size: 32px; color: var(--done);"></i>
        </div>
    </div>

    <div class="filters">
        <label><i class="ti ti-filter"></i> تصفية المهام:</label>
        <select id="typeFilter" onchange="renderTasks()">
            <option value="all">كل الأنواع (PM / CM)</option>
            <option value="PM">وقائية (PM)</option>
            <option value="CM">تصحيحية (CM)</option>
        </select>
        <select id="statusFilter" onchange="renderTasks()">
            <option value="all">كل الحالات</option>
            <option value="معلقة">معلقة</option>
            <option value="جارية">جارية</option>
            <option value="مكتملة">مكتملة</option>
        </select>
    </div>

    <div class="main-content">
        <div class="team-panel">
            <div class="panel-title"><i class="ti ti-users"></i> أعضاء الفريق</div>
            <div id="teamList">
                </div>
        </div>

        <div class="tasks-panel">
            <div class="panel-title"><i class="ti ti-checkbox"></i> قائمة المهام المسندة</div>
            <table class="tasks-table">
                <thead>
                    <tr>
                        <th>المهمة</th>
                        <th>النوع</th>
                        <th>المسؤول</th>
                        <th>الحالة</th>
                        <th class="close-task-btn-head" style="display:none;">إجراء</th>
                    </tr>
                </thead>
                <tbody id="tasksTableBody">
                    </tbody>
            </table>
        </div>
    </div>

    <footer>
        © 2026 Saudi Aramco. All rights reserved. <br>
        Version: 1.0 | Last Updated: 2026-05-28
    </footer>

    <script>
        // بيانات المهام الافتراضية للمشروع
        let tasks = [
            { id: 1, text: "فحص وتزييت التوربينات في القسم الشمالي", type: "PM", owner: "Yahya", status: "مكتملة" },
            { id: 2, text: "إصلاح تسريب صمام خط الأنابيب رقم 4", type: "CM", owner: "Abdulla", status: "جارية" },
            { id: 3, text: "تحديث نظام البرمجيات لغرفة التحكم الرئيسية", type: "PM", owner: "Ahmed", status: "معلقة" },
            { id: 4, text: "عايرة أجهزة قياس الضغط لخزانات الغاز", type: "PM", owner: "Ali", status: "جارية" },
            { id: 5, text: "استبدال كابلات الشبكة التالفة في القطاع ب", type: "CM", owner: "Mohamd", status: "معلقة" },
            { id: 6, text: "الاختبار الدوري لأنظمة الإطفاء الآلية", type: "PM", owner: "Yahya", status: "جارية" }
        ];

        // أسماء أعضاء الفريق ومسمياتهم
        const team = [
            { name: "Yahya", role: "Team Lead" },
            { name: "Abdulla", role: "Engineer" },
            { name: "Ahmed", role: "Technician" },
            { name: "Ali", role: "Engineer" },
            { name: "Mohamd", role: "Technician" }
        ];

        let isAdmin = false;
        let selectedMember = 'all';

        // تشغيل التطبيق وتحديث البيانات لأول مرة
        window.onload = function() {
            renderTeam();
            calculateStats();
            renderTasks();
        };

        // توليد قائمة الموظفين بالجانب
        function renderTeam() {
            const teamList = document.getElementById('teamList');
            let html = `<div class="member-item active" id="member-all" onclick="selectMember('all')">
                <div class="member-avatar"><i class="ti ti-apps"></i></div>
                <div class="member-info"><h4>الكل</h4><p>عرض جميع المهام</p></div>
            </div>`;

            team.forEach(m => {
                // حساب مهام كل موظف
                let count = tasks.filter(t => t.owner === m.name).length;
                html += `
                    <div class="member-item" id="member-${m.name}" onclick="selectMember('${m.name}')">
                        <div class="member-avatar">${m.name.substring(0,2)}</div>
                        <div class="member-info">
                            <h4>${m.name}</h4>
                            <p>${m.role} (${count} مهام)</p>
                        </div>
                    </div>`;
            });
            teamList.innerHTML = html;
        }

        // اختيار موظف لتصفية مهامه
        function selectMember(name) {
            document.querySelectorAll('.member-item').forEach(el => el.classList.remove('active'));
            document.getElementById(`member-${name}`).classList.add('active');
            selectedMember = name;
            renderTasks();
        }

        // حساب وتحديث الإحصائيات العلوية مباشرة
        function calculateStats() {
            document.getElementById('totalTasks').innerText = tasks.length;
            document.getElementById('pendingTasks').innerText = tasks.filter(t => t.status === 'معلقة').length;
            document.getElementById('progressTasks').innerText = tasks.filter(t => t.status === 'جارية').length;
            document.getElementById('doneTasks').innerText = tasks.filter(t => t.status === 'مكتملة').length;
        }

        // عرض وجدولة المهام بناءً على الفلاتر والموظف المحدد
        function renderTasks() {
            const tbody = document.getElementById('tasksTableBody');
            const typeFilter = document.getElementById('typeFilter').value;
            const statusFilter = document.getElementById('statusFilter').value;
            
            let filteredTasks = tasks.filter(t => {
                let matchMember = (selectedMember === 'all' || t.owner === selectedMember);
                let matchType = (typeFilter === 'all' || t.type === typeFilter);
                let matchStatus = (statusFilter === 'all' || t.status === statusFilter);
                return matchMember && matchType && matchStatus;
            });

            let html = '';
            if(filteredTasks.length === 0) {
                html = '<tr><td colspan="5" style="text-align:center; color:#999;">لا توجد مهام مطابقة للبحث.</td></tr>';
            } else {
                filteredTasks.forEach(t => {
                    let statusClass = t.status === 'معلقة' ? 'status-pending' : (t.status === 'جارية' ? 'status-progress' : 'status-done');
                    html += `
                        <tr>
                            <td>${t.text}</td>
                            <td><span class="badge ${t.type.toLowerCase()}">${t.type}</span></td>
                            <td><strong>${t.owner}</strong></td>
                            <td><span class="status-dot ${statusClass}">${t.status}</span></td>
                            <td><button class="close-task-btn" onclick="closeTask(${t.id})">إكمال</button></td>
                        </tr>`;
                });
            }
            tbody.innerHTML = html;
        }

        // خاصية دخول المدير وتفعيل قفل كلمة المرور
        function toggleAdmin() {
            if (!isAdmin) {
                let password = prompt("الرجاء إدخال كلمة مرور المدير لإغلاق المهام:");
                if (password === "aramco2025") {
                    isAdmin = true;
                    document.body.classList.add('admin-mode');
                    document.querySelector('.close-task-btn-head').style.display = 'table-cell';
                    document.getElementById('adminBtnText').innerText = "خروج المدير";
                    document.getElementById('adminBtn').classList.add('logged-in');
                    renderTasks(); // لإعادة البناء مع الأزرار
                } else {
                    alert("كلمة المرور خاطئة!");
                }
            } else {
                isAdmin = false;
                document.body.classList.remove('admin-mode');
                document.querySelector('.close-task-btn-head').style.display = 'none';
                document.getElementById('adminBtnText').innerText = "دخول المدير";
                document.getElementById('adminBtn').classList.remove('logged-in');
                renderTasks();
            }
        }

        // دالة إنهاء وإكمال المهمة من قبل المدير
        function closeTask(id) {
            let taskIndex = tasks.findIndex(t => t.id === id);
            if(taskIndex !== -1) {
                tasks[taskIndex].status = "مكتملة";
                calculateStats();
                renderTeam(); // لتحديث عدّاد المهام عند الاسم
                renderTasks();
            }
        }
    </script>
</body>
</html>
