# Day 18: Build a Brain Dump Action Planner Skill

## 🎯 Overview
Today, I built a custom Claude Skill named `brain-dump-action-planner`. This skill automates the process of transforming chaotic, unstructured information—like raw meeting transcripts, stream-of-consciousness brain dumps, or voice memos—into beautifully formatted, high-fidelity project dashboards without making assumptions or inventing missing data.

---

## 🛠️ Custom Skill Setup

- **Skill Name:** `brain-dump-action-planner`
- **Description:** Transform messy notes, meeting transcripts, voice memos, brainstorming sessions, and stream-of-consciousness thoughts into structured summaries, action plans, decisions, open questions, and task lists. Organize information clearly without inventing, assuming, or filling gaps. Preserve all names, dates, numbers, and terminology exactly as provided.

### 📸 Skill Configuration Screenshot
![Claude Custom Skill Configuration](path/to/your/skill-setup-screenshot.png) 
*(Note: Replace this placeholder with your actual screenshot of the custom skill saved in Claude)*

---

## 📊 Sample Input & Generated Dashboard

### 📝 Test Input (Example Meeting Notes)
> "Project Alpha sync. Sarah needs to finish the API design by next Tuesday, but she's waiting on John to provide the database schema. John says he'll try by Friday but there's a risk the schema might change later. Also, we aren't sure who is handling the frontend deployment yet. Budget is fixed at $5,000."

### 💻 Generated Dashboard Output (HTML Artifact)
Below is a simulation of the generated responsive dashboard layout produced by the skill:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <style>
        body { font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif; background-color: #f8f9fa; color: #333; padding: 20px; }
        .dashboard { max-width: 900px; margin: 0 auto; background: white; padding: 25px; border-radius: 8px; box-shadow: 0 4px 6px rgba(0,0,0,0.05); }
        h1 { color: #111; border-bottom: 2px solid #eaeaea; padding-bottom: 10px; }
        .card { background: #fff; border: 1px solid #e1e4e8; border-radius: 6px; padding: 15px; margin-bottom: 15px; }
        .badge { display: inline-block; padding: 4px 8px; border-radius: 4px; font-size: 12px; font-weight: bold; }
        .high { background: #ffebe9; color: #cf222e; }
        .open { background: #fff8c5; color: #9a6700; }
        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th, td { text-align: left; padding: 10px; border-bottom: 1px solid #ddd; }
        th { background-color: #f6f8fa; }
    </style>
</head>
<body>
    <div class="dashboard">
        <h1>Project Alpha Sync Dashboard</h1>
        
        <div class="card">
            <h3>1. Summary</h3>
            <p>Brief status synchronization regarding Project Alpha covering API design, database schema dependencies, and unresolved frontend deployment ownership.</p>
        </div>

        <div class="card">
            <h3>2. Key Takeaways</h3>
            <ul>
                <li>API design is blocked by the database schema timeline.</li>
                <li>Fixed budget of $5,000 is established.</li>
            </ul>
        </div>

        <div class="card">
            <h3>3. Action Items</h3>
            <table>
                <tr><th>Task</th><th>Owner</th><th>Deadline</th><th>Status</th></tr>
                <tr><td>Finish API design</td><td>Sarah</td><td>Next Tuesday</td><td>⏳ Pending</td></tr>
                <tr><td>Provide database schema</td><td>John</td><td>Friday</td><td>⏳ Pending</td></tr>
            </table>
        </div>

        <div class="card">
            <h3>4. Open Questions</h3>
            <p><span class="badge open">❓ Open Question</span> Who is handling the frontend deployment?</p>
        </div>

        <div class="card">
            <h3>5. Risks / Blockers</h3>
            <p><span class="badge high">🔴 High Priority</span> Database schema might change later, introducing downstream technical risks.</p>
        </div>
    </div>
</body>
</html>