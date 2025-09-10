# Simba-Family-tree-App
Family tree app 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Family Tree Builder</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: #333;
            min-height: 100vh;
            padding: 20px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            overflow: hidden;
        }
        
        header {
            background: #4a6fc7;
            color: white;
            padding: 20px;
            text-align: center;
            position: relative;
        }
        
        h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        .header-controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 15px;
            flex-wrap: wrap;
        }
        
        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 50px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }
        
        .btn-primary {
            background: #ff7e5f;
            color: white;
        }
        
        .btn-primary:hover {
            background: #ff6347;
            transform: translateY(-2px);
        }
        
        .btn-secondary {
            background: #2bc0a9;
            color: white;
        }
        
        .btn-secondary:hover {
            background: #22a892;
            transform: translateY(-2px);
        }
        
        .btn-outline {
            background: transparent;
            border: 2px solid white;
            color: white;
        }
        
        .btn-outline:hover {
            background: white;
            color: #4a6fc7;
        }
        
        .connection-status {
            position: absolute;
            top: 20px;
            right: 20px;
            display: flex;
            align-items: center;
            gap: 8px;
            background: rgba(255, 255, 255, 0.2);
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.9rem;
        }
        
        .main-content {
            display: flex;
            min-height: 500px;
        }
        
        .sidebar {
            width: 300px;
            background: #f5f7fa;
            padding: 20px;
            border-right: 1px solid #e0e6ed;
        }
        
        .search-box {
            margin-bottom: 20px;
        }
        
        .search-box input {
            width: 100%;
            padding: 10px 15px;
            border: 1px solid #ddd;
            border-radius: 50px;
            font-size: 1rem;
        }
        
        .family-list {
            list-style: none;
        }
        
        .family-list li {
            padding: 12px 15px;
            margin-bottom: 8px;
            background: white;
            border-radius: 8px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 10px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
            transition: all 0.2s ease;
        }
        
        .family-list li:hover {
            background: #e9f2ff;
            transform: translateX(5px);
        }
        
        .family-list li.active {
            background: #d4e5ff;
            border-left: 4px solid #4a6fc7;
        }
        
        .tree-container {
            flex: 1;
            padding: 20px;
            overflow: auto;
            position: relative;
        }
        
        .tree {
            display: flex;
            justify-content: center;
            padding: 40px 0;
            min-height: 100%;
        }
        
        .person {
            width: 120px;
            background: white;
            border-radius: 10px;
            padding: 15px;
            margin: 10px;
            text-align: center;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
            position: relative;
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 2;
        }
        
        .person:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
        }
        
        .person.male {
            border-bottom: 4px solid #4a6fc7;
        }
        
        .person.female {
            border-bottom: 4px solid #ff7e5f;
        }
        
        .person .avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            margin: 0 auto 10px;
            overflow: hidden;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2rem;
            color: white;
        }
        
        .person.male .avatar {
            background: #4a6fc7;
        }
        
        .person.female .avatar {
            background: #ff7e5f;
        }
        
        .person .name {
            font-weight: 600;
            margin-bottom: 5px;
        }
        
        .person .details {
            font-size: 0.8rem;
            color: #666;
        }
        
        .connections {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            pointer-events: none;
        }
        
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 100;
            align-items: center;
            justify-content: center;
        }
        
        .modal-content {
            background: white;
            border-radius: 15px;
            padding: 30px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .close {
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
        }
        
        .form-group input, .form-group select {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
        }
        
        .form-actions {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            margin-top: 30px;
        }
        
        footer {
            text-align: center;
            padding: 20px;
            background: #f5f7fa;
            color: #666;
            font-size: 0.9rem;
        }
        
        @media (max-width: 768px) {
            .main-content {
                flex-direction: column;
            }
            
            .sidebar {
                width: 100%;
                border-right: none;
                border-bottom: 1px solid #e0e6ed;
            }
            
            .header-controls {
                flex-direction: column;
                align-items: center;
            }
            
            .btn {
                width: 100%;
                justify-content: center;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>Family Tree Builder</h1>
            <p>Create, visualize, and share your family history</p>
            <div class="connection-status">
                <i class="fas fa-wifi"></i>
                <span>Online</span>
            </div>
            <div class="header-controls">
                <button class="btn btn-primary" id="addPersonBtn">
                    <i class="fas fa-plus"></i> Add Person
                </button>
                <button class="btn btn-secondary" id="shareTreeBtn">
                    <i class="fas fa-share-alt"></i> Share Tree
                </button>
                <button class="btn btn-outline" id="saveTreeBtn">
                    <i class="fas fa-save"></i> Save Locally
                </button>
                <button class="btn btn-outline" id="importTreeBtn">
                    <i class="fas fa-file-import"></i> Import
                </button>
            </div>
        </header>
        
        <div class="main-content">
            <div class="sidebar">
                <div class="search-box">
                    <input type="text" placeholder="Search family members...">
                </div>
                
                <h3>Family Members</h3>
                <ul class="family-list">
                    <li class="active">
                        <i class="fas fa-user"></i> John Smith (Grandfather)
                    </li>
                    <li>
                        <i class="fas fa-user"></i> Mary Smith (Grandmother)
                    </li>
                    <li>
                        <i class="fas fa-user"></i> Robert Smith (Father)
                    </li>
                    <li>
                        <i class="fas fa-user"></i> Jennifer Smith (Mother)
                    </li>
                    <li>
                        <i class="fas fa-user"></i> Michael Smith (Son)
                    </li>
                    <li>
                        <i class="fas fa-user"></i> Sarah Smith (Daughter)
                    </li>
                </ul>
            </div>
            
            <div class="tree-container">
                <div class="tree">
                    <!-- Top row (grandparents) -->
                    <div class="person male">
                        <div class="avatar">JS</div>
                        <div class="name">John Smith</div>
                        <div class="details">b. 1945</div>
                    </div>
                    
                    <div class="person female">
                        <div class="avatar">MS</div>
                        <div class="name">Mary Smith</div>
                        <div class="details">b. 1948</div>
                    </div>
                    
                    <!-- Middle row (parents) -->
                    <div class="person male">
                        <div class="avatar">RS</div>
                        <div class="name">Robert Smith</div>
                        <div class="details">b. 1970</div>
                    </div>
                    
                    <div class="person female">
                        <div class="avatar">JS</div>
                        <div class="name">Jennifer Smith</div>
                        <div class="details">b. 1972</div>
                    </div>
                    
                    <!-- Bottom row (children) -->
                    <div class="person male">
                        <div class="avatar">MS</div>
                        <div class="name">Michael Smith</div>
                        <div class="details">b. 2000</div>
                    </div>
                    
                    <div class="person female">
                        <div class="avatar">SS</div>
                        <div class="name">Sarah Smith</div>
                        <div class="details">b. 2003</div>
                    </div>
                    
                    <!-- Connection lines -->
                    <div class="connections">
                        <svg width="100%" height="100%" style="position: absolute; top: 0; left: 0;">
                            <!-- Lines connecting grandparents to parents -->
                            <line x1="70" y1="100" x2="270" y2="200" stroke="#4a6fc7" stroke-width="2" />
                            <line x1="190" y1="100" x2="270" y2="200" stroke="#4a6fc7" stroke-width="2" />
                            
                            <!-- Lines connecting parents to children -->
                            <line x1="270" y1="250" x2="370" y2="350" stroke="#4a6fc7" stroke-width="2" />
                            <line x1="270" y1="250" x2="470" y2="350" stroke="#4a6fc7" stroke-width="2" />
                        </svg>
                    </div>
                </div>
            </div>
        </div>
        
        <footer>
            <p>Family Tree Builder &copy; 2023 | Works offline | Export and share with family</p>
        </footer>
    </div>
    
    <!-- Add Person Modal -->
    <div class="modal" id="addPersonModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Add Family Member</h2>
                <span class="close">&times;</span>
            </div>
            
            <div class="form-group">
                <label for="personName">Full Name</label>
                <input type="text" id="personName" placeholder="Enter full name">
            </div>
            
            <div class="form-group">
                <label for="personGender">Gender</label>
                <select id="personGender">
                    <option value="male">Male</option>
                    <option value="female">Female</option>
                    <option value="other">Other</option>
                </select>
            </div>
            
            <div class="form-group">
                <label for="birthYear">Year of Birth</label>
                <input type="number" id="birthYear" placeholder="Enter year of birth">
            </div>
            
            <div class="form-group">
                <label for="relation">Relation</label>
                <select id="relation">
                    <option value="parent">Parent</option>
                    <option value="spouse">Spouse</option>
                    <option value="child">Child</option>
                    <option value="sibling">Sibling</option>
                </select>
            </div>
            
            <div class="form-group">
                <label for="relatedTo">Related To</label>
                <select id="relatedTo">
                    <option value="johnSmith">John Smith</option>
                    <option value="marySmith">Mary Smith</option>
                    <option value="robertSmith">Robert Smith</option>
                </select>
            </div>
            
            <div class="form-actions">
                <button class="btn btn-outline" style="color: #333; border-color: #ddd;">Cancel</button>
                <button class="btn btn-primary">Add Person</button>
            </div>
        </div>
    </div>

    <script>
        // Modal functionality
        const modal = document.getElementById('addPersonModal');
        const addPersonBtn = document.getElementById('addPersonBtn');
        const closeBtn = document.querySelector('.close');
        
        addPersonBtn.addEventListener('click', () => {
            modal.style.display = 'flex';
        });
        
        closeBtn.addEventListener('click', () => {
            modal.style.display = 'none';
        });
        
        window.addEventListener('click', (e) => {
            if (e.target === modal) {
                modal.style.display = 'none';
            }
        });
        
        // Simulate offline/online status
        setInterval(() => {
            const status = document.querySelector('.connection-status');
            const random = Math.random();
            
            if (random < 0.1) {
                status.innerHTML = '<i class="fas fa-wifi-slash"></i> Offline';
                status.style.background = '#ff7e5f';
            } else {
                status.innerHTML = '<i class="fas fa-wifi"></i> Online';
                status.style.background = 'rgba(255, 255, 255, 0.2)';
            }
        }, 5000);
        
        // Save tree button functionality
        document.getElementById('saveTreeBtn').addEventListener('click', () => {
            alert('Family tree saved locally! It will be available when offline.');
        });
        
        // Share tree button functionality
        document.getElementById('shareTreeBtn').addEventListener('click', () => {
            alert('Shareable link copied to clipboard!\nhttp://familytree.app/share/abc123');
        });
        
        // Import tree button functionality
        document.getElementById('importTreeBtn').addEventListener('click', () => {
            alert('Select a family tree file to import...');
        });
        
        // Person click functionality
        const people = document.querySelectorAll('.person');
        people.forEach(person => {
            person.addEventListener('click', () => {
                const name = person.querySelector('.name').textContent;
                alert(`Selected: ${name}\nWould you like to view or edit details?`);
            });
        });
    </script>
</body>
</html>  
