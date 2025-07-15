name: "Progress Dashboard PRP"
description: |

## Purpose
Create an enhanced progress dashboard system that provides real-time visibility into PRP execution progress, including visual displays, time tracking, completion metrics, and integration with the execution workflow.

## Core Principles
1. **Context is King**: Include ALL necessary documentation, examples, and caveats
2. **Validation Loops**: Provide executable tests/lints the AI can run and fix
3. **Information Dense**: Use keywords and patterns from the codebase
4. **Progressive Success**: Start simple, validate, then enhance
5. **Global rules**: Be sure to follow all rules in CLAUDE.md

---

## Goal
Build an enhanced progress dashboard that goes beyond the basic implementation to provide comprehensive execution monitoring, analytics, predictions, and visual reporting for PRP executions.

## Why
- Real-time progress visibility improves success rates
- Time tracking enables better estimates
- Visual dashboards motivate completion
- Analytics identify bottlenecks
- Predictions help with planning

## What
Create enhanced dashboard with:
- Real-time progress updates
- Visual progress displays
- Time tracking and predictions
- Execution analytics
- Multi-PRP tracking
- Export capabilities
- Web-based interface option

### Success Criteria
- [ ] Dashboard shows real-time progress
- [ ] Visual elements enhance understanding
- [ ] Time predictions are accurate
- [ ] Analytics provide insights
- [ ] Multi-PRP view available
- [ ] Export functionality works
- [ ] Integration seamless

## All Needed Context

### Documentation & References
```yaml
# MUST READ - Include these in your context window
- file: docs/scripts/progress_dashboard.py
  why: Basic dashboard already exists to enhance
  
- file: docs/guides/execute_prp_quick_start.md
  why: Dashboard integration mentioned
  
- file: docs/templates/execution_tracking_template.md
  why: Data source for dashboard

- file: .claude/commands/execute-prp.md
  why: Integration points

- file: CLAUDE.md
  why: Progress tracking requirements
```

### Current Codebase tree
```bash
context-engineering-intro/
├── docs/scripts/progress_dashboard.py  # Basic version exists
└── (no enhanced dashboard)
```

### Desired Codebase tree with files to be added
```bash
context-engineering-intro/
├── docs/
│   ├── scripts/
│   │   ├── progress_dashboard.py       # Enhanced version
│   │   ├── dashboard_server.py        # Web interface
│   │   └── dashboard_cli.py          # CLI enhancements
│   └── dashboards/
│       ├── templates/
│       │   ├── dashboard.html         # Web UI template
│       │   └── style.css             # Dashboard styles
│       └── static/
│           └── dashboard.js          # Interactive features
```

### Known Gotchas
```python
# CRITICAL: Dashboard must work without dependencies
# Core functionality with just Python stdlib

# CRITICAL: Real-time updates without complexity
# Use file watching or polling

# CRITICAL: Terminal and web versions
# Different users prefer different interfaces

# CRITICAL: Performance with many PRPs
# Don't slow down execution
```

## Implementation Blueprint

### Task Order

```yaml
Task 1 - Enhance core dashboard:
ENHANCE docs/scripts/progress_dashboard.py:
  - ADD: Visual progress bars
  - IMPLEMENT: Time predictions
  - CREATE: Analytics functions
  - SUPPORT: Multi-PRP tracking

Task 2 - Create CLI enhancements:
CREATE docs/scripts/dashboard_cli.py:
  - IMPLEMENT: Rich terminal UI
  - ADD: Interactive controls
  - CREATE: Export functions
  - SUPPORT: Watch mode

Task 3 - Create web dashboard:
CREATE docs/scripts/dashboard_server.py:
  - IMPLEMENT: Flask/FastAPI server
  - CREATE: REST endpoints
  - ADD: WebSocket support
  - SERVE: Dashboard UI

Task 4 - Create web UI:
CREATE docs/dashboards/templates/*:
  - DESIGN: Responsive layout
  - IMPLEMENT: Real-time updates
  - ADD: Interactive charts
  - CREATE: Export options
```

### Per task pseudocode

```python
# Task 1 - Enhanced progress_dashboard.py
#!/usr/bin/env python3
"""Enhanced PRP Execution Progress Dashboard with analytics and predictions."""

import time
import json
import statistics
from datetime import datetime, timedelta
from pathlib import Path
import sys
from collections import defaultdict
from typing import Dict, List, Optional, Tuple
import curses

class EnhancedProgressDashboard:
    """Advanced progress tracking with predictions and analytics."""
    
    def __init__(self, prp_name: str = "current"):
        self.prp_name = prp_name
        self.progress_file = Path(f".progress_{prp_name}.json")
        self.history_file = Path(".execution_history.json")
        self.steps = [
            ("Load Context", 5, "📖"),
            ("Pattern Match", 5, "🔍"),
            ("Scaffold", 10, "🏗️"),
            ("Core Sprint", 45, "💻"),
            ("Test & Fix", 15, "🧪"),
            ("Polish", 10, "✨")
        ]
        self.load_data()
    
    def load_data(self):
        """Load progress and historical data."""
        # Current progress
        if self.progress_file.exists():
            with open(self.progress_file) as f:
                self.data = json.load(f)
        else:
            self.data = {
                "started": datetime.now().isoformat(),
                "prp_name": self.prp_name,
                "steps": {
                    name: {
                        "complete": 0,
                        "time_spent": 0,
                        "started_at": None,
                        "completed_at": None
                    } for name, _, _ in self.steps
                }
            }
        
        # Historical data for predictions
        if self.history_file.exists():
            with open(self.history_file) as f:
                self.history = json.load(f)
        else:
            self.history = {"executions": []}
    
    def update_step(self, step_name: str, percent_complete: int):
        """Update step progress with timing."""
        if step_name in self.data["steps"]:
            step = self.data["steps"][step_name]
            
            # Track timing
            if percent_complete > 0 and step["started_at"] is None:
                step["started_at"] = datetime.now().isoformat()
            
            if percent_complete == 100 and step["completed_at"] is None:
                step["completed_at"] = datetime.now().isoformat()
                
                # Calculate actual time spent
                start = datetime.fromisoformat(step["started_at"])
                end = datetime.fromisoformat(step["completed_at"])
                step["time_spent"] = (end - start).total_seconds() / 60
            
            step["complete"] = percent_complete
            self.save_progress()
    
    def get_predictions(self) -> Dict[str, float]:
        """Predict completion times based on history."""
        predictions = {}
        
        if not self.history["executions"]:
            # Use default estimates
            for name, minutes, _ in self.steps:
                predictions[name] = minutes
        else:
            # Calculate averages from history
            step_times = defaultdict(list)
            
            for execution in self.history["executions"][-10:]:  # Last 10
                for step_name, step_data in execution["steps"].items():
                    if step_data["time_spent"] > 0:
                        step_times[step_name].append(step_data["time_spent"])
            
            for name, _, _ in self.steps:
                if name in step_times and step_times[name]:
                    # Use median for robustness
                    predictions[name] = statistics.median(step_times[name])
                else:
                    # Fallback to default
                    default = next((m for n, m, _ in self.steps if n == name), 10)
                    predictions[name] = default
        
        return predictions
    
    def get_analytics(self) -> Dict[str, any]:
        """Generate execution analytics."""
        analytics = {
            "current_efficiency": 0,
            "average_duration": 0,
            "success_rate": 0,
            "common_bottlenecks": [],
            "improvement_trend": []
        }
        
        if not self.history["executions"]:
            return analytics
        
        # Success rate
        completed = sum(1 for e in self.history["executions"] 
                       if e.get("completed", False))
        analytics["success_rate"] = (completed / len(self.history["executions"])) * 100
        
        # Average duration
        durations = []
        for execution in self.history["executions"]:
            if execution.get("total_time"):
                durations.append(execution["total_time"])
        
        if durations:
            analytics["average_duration"] = statistics.mean(durations)
        
        # Bottlenecks (steps that take longest)
        step_times = defaultdict(list)
        for execution in self.history["executions"]:
            for step_name, step_data in execution["steps"].items():
                if step_data["time_spent"] > 0:
                    step_times[step_name].append(step_data["time_spent"])
        
        bottlenecks = []
        for step_name, times in step_times.items():
            if times:
                avg_time = statistics.mean(times)
                expected = next((m for n, m, _ in self.steps if n == step_name), 10)
                if avg_time > expected * 1.5:  # 50% over estimate
                    bottlenecks.append({
                        "step": step_name,
                        "average": avg_time,
                        "expected": expected,
                        "overhead": ((avg_time - expected) / expected) * 100
                    })
        
        analytics["common_bottlenecks"] = sorted(
            bottlenecks, 
            key=lambda x: x["overhead"], 
            reverse=True
        )[:3]
        
        return analytics
    
    def render_visual(self) -> str:
        """Render visual progress display."""
        output = []
        predictions = self.get_predictions()
        analytics = self.get_analytics()
        
        # Header
        output.append(f"\n{'='*60}")
        output.append(f"🚀 PRP Execution Dashboard: {self.prp_name}")
        output.append(f"{'='*60}\n")
        
        # Overall progress bar
        total_complete = self._calculate_overall_progress()
        overall_bar = self._create_progress_bar(total_complete, 40)
        output.append(f"Overall Progress: {overall_bar} {total_complete}%")
        
        # Time tracking
        elapsed = self._get_elapsed_time()
        estimated_total = sum(predictions.values())
        eta = self._calculate_eta(total_complete, elapsed, estimated_total)
        
        output.append(f"Time: {self._format_time(elapsed)} / ~{self._format_time(estimated_total)}")
        if eta:
            output.append(f"ETA: {self._format_time(eta)}")
        output.append("")
        
        # Step details
        output.append("Step Progress:")
        output.append("-" * 60)
        
        for step_name, expected_minutes, emoji in self.steps:
            step_data = self.data["steps"][step_name]
            complete = step_data["complete"]
            actual_time = step_data["time_spent"]
            predicted_time = predictions[step_name]
            
            # Progress bar
            bar = self._create_progress_bar(complete, 20)
            
            # Status
            if complete == 100:
                status = "✅"
                time_info = f"{actual_time:.1f}m"
                if actual_time > predicted_time * 1.2:
                    time_info += " ⚠️"  # Slower than expected
            elif complete > 0:
                status = "⏳"
                time_info = f"~{predicted_time:.0f}m"
            else:
                status = "⏸️"
                time_info = f"~{predicted_time:.0f}m"
            
            output.append(
                f"{emoji} {status} {step_name:<20} {bar} {complete:>3}% {time_info:>8}"
            )
        
        # Analytics section
        if analytics["success_rate"] > 0:
            output.append(f"\n{'='*60}")
            output.append("📊 Analytics")
            output.append("-" * 60)
            output.append(f"Success Rate: {analytics['success_rate']:.0f}%")
            output.append(f"Avg Duration: {self._format_time(analytics['average_duration'])}")
            
            if analytics["common_bottlenecks"]:
                output.append("\n⚠️  Common Bottlenecks:")
                for bottleneck in analytics["common_bottlenecks"]:
                    output.append(
                        f"  - {bottleneck['step']}: "
                        f"+{bottleneck['overhead']:.0f}% slower than expected"
                    )
        
        output.append(f"\n{'='*60}\n")
        
        return "\n".join(output)
    
    def _create_progress_bar(self, percent: float, width: int) -> str:
        """Create a visual progress bar."""
        filled = int(width * percent / 100)
        
        # Use different characters for different completion levels
        if percent == 100:
            bar_char = "█"
            empty_char = " "
        elif percent > 75:
            bar_char = "▓"
            empty_char = "░"
        elif percent > 50:
            bar_char = "▒"
            empty_char = "░"
        else:
            bar_char = "░"
            empty_char = " "
        
        bar = bar_char * filled + empty_char * (width - filled)
        return f"[{bar}]"
    
    def _calculate_overall_progress(self) -> int:
        """Calculate weighted overall progress."""
        total_weight = sum(minutes for _, minutes, _ in self.steps)
        weighted_complete = 0
        
        for step_name, expected_minutes, _ in self.steps:
            complete = self.data["steps"][step_name]["complete"]
            weight = expected_minutes / total_weight
            weighted_complete += complete * weight
        
        return int(weighted_complete)
    
    def _get_elapsed_time(self) -> float:
        """Get total elapsed time in minutes."""
        start = datetime.fromisoformat(self.data["started"])
        return (datetime.now() - start).total_seconds() / 60
    
    def _calculate_eta(self, percent_complete: float, elapsed: float, estimated_total: float) -> Optional[float]:
        """Calculate estimated time to completion."""
        if percent_complete == 0 or percent_complete == 100:
            return None
        
        # Use both elapsed time and estimates
        rate = elapsed / percent_complete
        remaining_percent = 100 - percent_complete
        eta_by_rate = remaining_percent * rate
        
        # Weight by how far we are
        weight = percent_complete / 100
        eta = (eta_by_rate * weight) + (estimated_total - elapsed) * (1 - weight)
        
        return max(0, eta)
    
    def _format_time(self, minutes: float) -> str:
        """Format time in human-readable format."""
        if minutes < 60:
            return f"{int(minutes)}m"
        else:
            hours = int(minutes // 60)
            mins = int(minutes % 60)
            return f"{hours}h {mins}m"
    
    def save_progress(self):
        """Save current progress."""
        with open(self.progress_file, 'w') as f:
            json.dump(self.data, f, indent=2)
    
    def complete_execution(self):
        """Mark execution as complete and save to history."""
        # Calculate total time
        total_time = self._get_elapsed_time()
        
        # Add to history
        execution_record = {
            "prp_name": self.prp_name,
            "started": self.data["started"],
            "completed": datetime.now().isoformat(),
            "total_time": total_time,
            "steps": self.data["steps"],
            "completed": True
        }
        
        self.history["executions"].append(execution_record)
        
        # Keep only last 50 executions
        self.history["executions"] = self.history["executions"][-50:]
        
        # Save history
        with open(self.history_file, 'w') as f:
            json.dump(self.history, f, indent=2)
    
    def export_report(self, format: str = "markdown") -> str:
        """Export execution report."""
        if format == "markdown":
            return self._export_markdown()
        elif format == "json":
            return json.dumps({
                "current": self.data,
                "analytics": self.get_analytics(),
                "predictions": self.get_predictions()
            }, indent=2)
        else:
            raise ValueError(f"Unknown format: {format}")
    
    def _export_markdown(self) -> str:
        """Export as markdown report."""
        report = []
        report.append(f"# PRP Execution Report: {self.prp_name}")
        report.append(f"\nGenerated: {datetime.now().strftime('%Y-%m-%d %H:%M')}")
        
        # Summary
        report.append("\n## Summary")
        total_complete = self._calculate_overall_progress()
        elapsed = self._get_elapsed_time()
        report.append(f"- Progress: {total_complete}%")
        report.append(f"- Elapsed Time: {self._format_time(elapsed)}")
        
        # Step details
        report.append("\n## Step Details")
        report.append("| Step | Progress | Time | Status |")
        report.append("|------|----------|------|--------|")
        
        for step_name, _, _ in self.steps:
            step = self.data["steps"][step_name]
            status = "✅ Complete" if step["complete"] == 100 else "🏃 In Progress" if step["complete"] > 0 else "⏸️ Pending"
            time = f"{step['time_spent']:.1f}m" if step["time_spent"] > 0 else "-"
            report.append(f"| {step_name} | {step['complete']}% | {time} | {status} |")
        
        # Analytics
        analytics = self.get_analytics()
        if analytics["success_rate"] > 0:
            report.append("\n## Historical Analytics")
            report.append(f"- Success Rate: {analytics['success_rate']:.0f}%")
            report.append(f"- Average Duration: {self._format_time(analytics['average_duration'])}")
        
        return "\n".join(report)

# CLI Interface
def main():
    """Enhanced CLI with visual display."""
    import argparse
    
    parser = argparse.ArgumentParser(description="Enhanced PRP Progress Dashboard")
    parser.add_argument("--name", default="current", help="PRP name")
    parser.add_argument("--update", nargs=2, metavar=("STEP", "PERCENT"),
                       help="Update step progress")
    parser.add_argument("--complete", action="store_true", 
                       help="Mark execution as complete")
    parser.add_argument("--export", choices=["markdown", "json"],
                       help="Export report")
    parser.add_argument("--watch", action="store_true",
                       help="Watch mode - auto refresh")
    
    args = parser.parse_args()
    
    dashboard = EnhancedProgressDashboard(args.name)
    
    if args.update:
        step_name, percent = args.update
        dashboard.update_step(step_name, int(percent))
        print(f"✅ Updated {step_name} to {percent}%")
    
    if args.complete:
        dashboard.complete_execution()
        print("✅ Execution marked as complete")
    
    if args.export:
        report = dashboard.export_report(args.export)
        print(report)
    elif args.watch:
        # Watch mode
        try:
            while True:
                print("\033[2J\033[H")  # Clear screen
                print(dashboard.render_visual())
                time.sleep(5)  # Refresh every 5 seconds
        except KeyboardInterrupt:
            print("\nExiting watch mode")
    else:
        # Single display
        print(dashboard.render_visual())

if __name__ == "__main__":
    main()

# Task 2 - dashboard_cli.py
#!/usr/bin/env python3
"""Rich terminal UI for progress dashboard."""

try:
    from rich.console import Console
    from rich.table import Table
    from rich.progress import Progress, SpinnerColumn, TextColumn
    from rich.live import Live
    from rich.layout import Layout
    from rich.panel import Panel
    RICH_AVAILABLE = True
except ImportError:
    RICH_AVAILABLE = False

class RichDashboard:
    """Terminal UI using Rich library."""
    
    def __init__(self, dashboard: EnhancedProgressDashboard):
        self.dashboard = dashboard
        self.console = Console()
    
    def render_live(self):
        """Live updating dashboard."""
        layout = self._create_layout()
        
        with Live(layout, refresh_per_second=1) as live:
            while True:
                layout["main"].update(self._create_main_panel())
                layout["analytics"].update(self._create_analytics_panel())
                time.sleep(1)
    
    def _create_layout(self) -> Layout:
        """Create dashboard layout."""
        layout = Layout()
        layout.split_column(
            Layout(name="header", size=3),
            Layout(name="main"),
            Layout(name="analytics", size=10)
        )
        
        layout["header"].update(
            Panel(f"🚀 PRP Execution: {self.dashboard.prp_name}", 
                  style="bold blue")
        )
        
        return layout

# Continue with more dashboard implementations...

# Task 3 - dashboard_server.py
"""Web server for dashboard."""

from flask import Flask, jsonify, render_template
from flask_socketio import SocketIO, emit
import threading

app = Flask(__name__)
socketio = SocketIO(app, cors_allowed_origins="*")

class DashboardServer:
    def __init__(self, dashboard: EnhancedProgressDashboard):
        self.dashboard = dashboard
        self.app = app
        self.socketio = socketio
        
    @app.route("/")
    def index():
        return render_template("dashboard.html")
    
    @app.route("/api/progress")
    def get_progress():
        return jsonify({
            "progress": dashboard._calculate_overall_progress(),
            "steps": dashboard.data["steps"],
            "analytics": dashboard.get_analytics()
        })
    
    def run(self, host="0.0.0.0", port=5000):
        # Start background thread for updates
        threading.Thread(target=self._emit_updates, daemon=True).start()
        
        self.socketio.run(self.app, host=host, port=port)
    
    def _emit_updates(self):
        """Emit progress updates via WebSocket."""
        while True:
            data = {
                "progress": self.dashboard._calculate_overall_progress(),
                "steps": self.dashboard.data["steps"]
            }
            self.socketio.emit("progress_update", data)
            time.sleep(2)
```

### Integration Points
```yaml
DASHBOARD_INTEGRATION:
  - enhances: Basic progress tracking
  - provides: Visual feedback
  - enables: Better monitoring
  
INTERFACES:
  - CLI: Enhanced terminal display
  - Web: Browser-based dashboard
  - API: REST endpoints for integration
```

## Validation Loop

### Level 1: Functionality Testing
```bash
# Test basic dashboard
python docs/scripts/progress_dashboard.py --name test

# Test updates
python docs/scripts/progress_dashboard.py --name test --update "Load Context" 50

# Test export
python docs/scripts/progress_dashboard.py --name test --export markdown
```

### Level 2: Visual Testing
```python
def test_progress_bars():
    '''Progress bars render correctly.'''
    dashboard = EnhancedProgressDashboard("test")
    
    # Test different percentages
    for percent in [0, 25, 50, 75, 100]:
        bar = dashboard._create_progress_bar(percent, 20)
        assert len(bar) == 22  # [bar]
        assert bar.startswith("[")
        assert bar.endswith("]")

def test_analytics():
    '''Analytics calculations work.'''
    dashboard = EnhancedProgressDashboard("test")
    analytics = dashboard.get_analytics()
    
    assert "success_rate" in analytics
    assert "average_duration" in analytics
    assert isinstance(analytics["common_bottlenecks"], list)
```

## Final validation Checklist
- [ ] Enhanced dashboard functional
- [ ] Visual progress bars work
- [ ] Time predictions accurate
- [ ] Analytics provide insights
- [ ] Export functionality works
- [ ] Web interface optional
- [ ] Performance acceptable
- [ ] Integration seamless

---

## Anti-Patterns to Avoid
- ❌ Don't require heavy dependencies
- ❌ Don't slow down execution
- ❌ Don't make UI too complex
- ❌ Don't forget fallbacks
- ❌ Don't ignore terminal limits
- ❌ Don't overcomplicate analytics