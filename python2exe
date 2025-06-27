import os
import subprocess
import platform
import tkinter as tk
from tkinter import ttk, messagebox, filedialog
import webbrowser

class StorageDashboard:
    def __init__(self, root):
        self.root = root
        self.root.title("STORAGER")
        self.root.geometry("800x600")
        self.root.configure(bg="#121212")
        self.root.resizable(True, True)
        
        # Set dark theme
        self.style = ttk.Style()
        self.style.theme_use("clam")
        self.configure_styles()
        
        # Initialize settings
        self.settings = {
            "theme": "Dark",
            "always_on_top": False
        }
        
        self.create_widgets()
        self.apply_theme()
        
    def configure_styles(self):
        # Background colors
        self.style.configure(".", background="#121212", foreground="#e0e0e0")
        self.style.configure("TFrame", background="#1e1e1e")
        self.style.configure("Header.TLabel", background="#1e1e1e", foreground="#4fc3f7", font=("Arial", 24, "bold"))
        
        # Define social button style with larger font
        self.style.configure("SocialBig.TButton", 
                             background="#1e1e1e", 
                             foreground="#bb86fc", 
                             borderwidth=0,
                             font=("Arial", 16, "bold"))
        
        self.style.configure("TButton", background="#333", foreground="#fff", borderwidth=1, font=("Arial", 10, "bold"))
        self.style.map("TButton", background=[("active", "#444")])
        self.style.configure("TCheckbutton", background="#1e1e1e", foreground="#e0e0e0")
        self.style.configure("TNotebook", background="#121212", borderwidth=0)
        self.style.configure("TNotebook.Tab", background="#333", foreground="#fff", padding=[10, 5], font=("Arial", 10, "bold"))
        self.style.map("TNotebook.Tab", background=[("selected", "#4fc3f7")])
        
    def create_widgets(self):
        # Create notebook for tabs
        self.notebook = ttk.Notebook(self.root, style="TNotebook")
        self.notebook.pack(fill="both", expand=True, padx=10, pady=10)
        
        # Dashboard Tab
        self.dashboard_frame = ttk.Frame(self.notebook, style="TFrame")
        self.notebook.add(self.dashboard_frame, text="Dashboard")
        
        # Settings Tab
        self.settings_frame = ttk.Frame(self.notebook, style="TFrame")
        self.notebook.add(self.settings_frame, text="Settings")
        
        # Tools Tab
        self.tools_frame = ttk.Frame(self.notebook, style="TFrame")
        self.notebook.add(self.tools_frame, text="Tools")
        
        # Create content
        self.create_dashboard()
        self.create_settings()
        self.create_tools()
        self.create_footer()
    
    def create_dashboard(self):
        # STORAGER Logo
        logo_frame = ttk.Frame(self.dashboard_frame, style="TFrame")
        logo_frame.pack(pady=(30, 20))
        
        ttk.Label(
            logo_frame, 
            text="STORAGER", 
            style="Header.TLabel"
        ).pack()
        
        # Tagline
        ttk.Label(
            logo_frame, 
            text="Storage Management Toolkit", 
            foreground="#bb86fc",
            background="#1e1e1e",
            font=("Arial", 12)
        ).pack(pady=(0, 30))
        
        # Social Buttons
        social_frame = ttk.Frame(self.dashboard_frame, style="TFrame")
        social_frame.pack(pady=20)
        
        # GitHub Button
        github_frame = ttk.Frame(social_frame, style="TFrame")
        github_frame.pack(side="left", padx=20, pady=10)
        
        ttk.Button(
            github_frame,
            text="GitHub\n\n🖥️",
            style="SocialBig.TButton",  # Use the new style
            command=lambda: self.open_link("https://github.com/STORAGERKIR/GUI/commits?author=STORAGERKIR")
        ).pack()
        
        # Discord Button
        discord_frame = ttk.Frame(social_frame, style="TFrame")
        discord_frame.pack(side="left", padx=20, pady=10)
        
        ttk.Button(
            discord_frame,
            text="Discord\n\n💬",
            style="SocialBig.TButton",  # Use the new style
            command=lambda: self.open_link("https://discord.gg/KnHaMu38Qv")
        ).pack()
        
        # Description
        desc_frame = ttk.Frame(self.dashboard_frame, style="TFrame")
        desc_frame.pack(fill="x", padx=50, pady=30)
        
        ttk.Label(
            desc_frame, 
            text="STORAGER is your all-in-one storage management solution with tools for\n"
                 "file conversion, system utilities, and cloud integration.",
            justify="center",
            background="#1e1e1e",
            foreground="#e0e0e0",
            font=("Arial", 11)
        ).pack()
    
    def create_settings(self):
        # Theme selector
        ttk.Label(
            self.settings_frame, 
            text="THEME SETTINGS", 
            style="Header.TLabel"
        ).pack(pady=(10, 20))
        
        theme_frame = ttk.Frame(self.settings_frame, style="TFrame")
        theme_frame.pack(fill="x", padx=50, pady=10)
        
        ttk.Label(
            theme_frame, 
            text="Select Theme:", 
            style="TLabel",
            font=("Arial", 11)
        ).pack(anchor="w", pady=(0, 10))
        
        self.theme_var = tk.StringVar(value=self.settings["theme"])
        themes = ["Dark", "Darker", "Midnight"]
        
        for theme in themes:
            rb = ttk.Radiobutton(
                theme_frame,
                text=theme,
                value=theme,
                variable=self.theme_var,
                style="TCheckbutton",
                command=self.change_theme
            )
            rb.pack(anchor="w", padx=20, pady=5)
        
        # Always on top setting
        top_frame = ttk.Frame(self.settings_frame, style="TFrame")
        top_frame.pack(fill="x", padx=50, pady=20)
        
        ttk.Label(
            top_frame, 
            text="WINDOW SETTINGS", 
            style="Header.TLabel"
        ).pack(anchor="w", pady=(0, 10))
        
        self.always_on_top_var = tk.BooleanVar(value=self.settings["always_on_top"])
        chk = ttk.Checkbutton(
            top_frame,
            text="Always On Top",
            variable=self.always_on_top_var,
            style="TCheckbutton",
            command=self.toggle_always_on_top
        )
        chk.pack(anchor="w", padx=20, pady=5)
        
        # Save button
        save_btn = ttk.Button(
            self.settings_frame,
            text="SAVE SETTINGS",
            command=self.save_settings,
            width=20
        )
        save_btn.pack(pady=30)
    
    def create_tools(self):
        # Python to EXE Section
        ttk.Label(
            self.tools_frame, 
            text="PYTHON TO EXE CONVERTER", 
            style="Header.TLabel"
        ).pack(pady=(10, 20))
        
        # File selection
        file_frame = ttk.Frame(self.tools_frame, style="TFrame")
        file_frame.pack(fill="x", padx=50, pady=10)
        
        ttk.Label(file_frame, text="Python File:").pack(anchor="w", pady=(0, 5))
        self.file_entry = ttk.Entry(file_frame)
        self.file_entry.pack(fill="x", pady=5)
        
        browse_frame = ttk.Frame(file_frame, style="TFrame")
        browse_frame.pack(fill="x", pady=10)
        
        ttk.Button(
            browse_frame,
            text="Browse",
            command=self.browse_python_file
        ).pack(side="right")
        
        # Options
        options_frame = ttk.Frame(self.tools_frame, style="TFrame")
        options_frame.pack(fill="x", padx=50, pady=10)
        
        self.onefile_var = tk.BooleanVar(value=True)
        ttk.Checkbutton(
            options_frame,
            text="Single Executable (--onefile)",
            variable=self.onefile_var,
            style="TCheckbutton"
        ).pack(anchor="w", pady=5)
        
        self.noconsole_var = tk.BooleanVar(value=False)
        ttk.Checkbutton(
            options_frame,
            text="No Console Window (--noconsole)",
            variable=self.noconsole_var,
            style="TCheckbutton"
        ).pack(anchor="w", pady=5)
        
        # Convert button
        ttk.Button(
            self.tools_frame,
            text="CONVERT TO EXE",
            command=self.convert_to_exe,
            width=20
        ).pack(pady=20)
        
        # System Tools Section
        ttk.Label(
            self.tools_frame, 
            text="SYSTEM TOOLS", 
            style="Header.TLabel"
        ).pack(pady=(20, 10))
        
        tools_frame = ttk.Frame(self.tools_frame, style="TFrame")
        tools_frame.pack(fill="x", padx=50, pady=10)
        
        tools = [
            ("Open CMD", self.open_cmd),
            ("System Info", self.show_system_info),
            ("Open Explorer", self.open_explorer)
        ]
        
        for tool in tools:
            ttk.Button(
                tools_frame,
                text=tool[0],
                command=tool[1],
                width=15
            ).pack(side="left", padx=5, pady=5)
    
    def create_footer(self):
        footer = ttk.Frame(self.root, style="TFrame")
        footer.pack(fill="x", side="bottom", pady=5)
        
        # Copyright
        ttk.Label(
            footer,
            text="© 2023 STORAGER | Storage Management Toolkit",
            foreground="#757575",
            background="#1e1e1e",
            font=("Arial", 9)
        ).pack(side="left", padx=10)
        
        # Version
        ttk.Label(
            footer,
            text="v1.0.0",
            foreground="#757575",
            background="#1e1e1e",
            font=("Arial", 9)
        ).pack(side="right", padx=10)
    
    def browse_python_file(self):
        file_path = filedialog.askopenfilename(
            filetypes=[("Python Files", "*.py"), ("All Files", "*.*")]
        )
        if file_path:
            self.file_entry.delete(0, tk.END)
            self.file_entry.insert(0, file_path)
    
    def convert_to_exe(self):
        file_path = self.file_entry.get()
        if not file_path:
            messagebox.showerror("Error", "Please select a Python file")
            return
            
        if not os.path.isfile(file_path):
            messagebox.showerror("Error", "File does not exist")
            return
            
        try:
            # Check if PyInstaller is installed
            subprocess.run(["pyinstaller", "--version"], check=True, 
                          stdout=subprocess.PIPE, stderr=subprocess.PIPE)
            
            # Build pyinstaller command
            cmd = ["pyinstaller"]
            
            if self.onefile_var.get():
                cmd.append("--onefile")
                
            if self.noconsole_var.get():
                cmd.append("--noconsole")
                
            cmd.append(file_path)
            
            # Run conversion
            subprocess.run(cmd, check=True)
            messagebox.showinfo(
                "Success", 
                f"Executable created in dist directory!\n"
                f"File: {os.path.basename(file_path).replace('.py', '.exe')}"
            )
        except FileNotFoundError:
            messagebox.showerror(
                "PyInstaller Not Found",
                "PyInstaller is not installed. Please install it using:\n"
                "pip install pyinstaller"
            )
        except subprocess.CalledProcessError as e:
            messagebox.showerror("Conversion Error", f"Failed to convert file: {e.stderr or e}")
        except Exception as e:
            messagebox.showerror("Conversion Error", str(e))
    
    def open_cmd(self):
        try:
            if platform.system() == "Windows":
                os.system("start cmd")
            elif platform.system() == "Darwin":
                subprocess.Popen(["open", "-a", "Terminal"])
            else:
                subprocess.Popen(["x-terminal-emulator"])
        except Exception as e:
            messagebox.showerror("Error", f"Could not open terminal: {str(e)}")
    
    def show_system_info(self):
        info = f"""
        System: {platform.system()} {platform.release()}
        Processor: {platform.processor() or 'Unknown'}
        Python Version: {platform.python_version()}
        Machine: {platform.machine()}
        """
        messagebox.showinfo("System Information", info.strip())
    
    def open_explorer(self):
        path = os.getcwd()
        try:
            if platform.system() == "Windows":
                os.startfile(path)
            elif platform.system() == "Darwin":
                subprocess.Popen(["open", path])
            else:
                subprocess.Popen(["xdg-open", path])
        except Exception as e:
            messagebox.showerror("Error", f"Could not open explorer: {str(e)}")
    
    def open_link(self, url):
        try:
            webbrowser.open_new(url)
        except Exception as e:
            messagebox.showerror("Error", f"Could not open URL: {str(e)}")
    
    def save_settings(self):
        self.settings = {
            "theme": self.theme_var.get(),
            "always_on_top": self.always_on_top_var.get()
        }
        messagebox.showinfo("Settings Saved", "All settings have been saved successfully")
    
    def toggle_always_on_top(self):
        self.root.attributes("-topmost", self.always_on_top_var.get())
    
    def change_theme(self):
        theme = self.theme_var.get()
        self.settings["theme"] = theme
        self.apply_theme()
    
    def apply_theme(self):
        theme = self.settings["theme"]
        
        if theme == "Dark":
            bg = "#121212"
            frame_bg = "#1e1e1e"
            accent = "#4fc3f7"
        elif theme == "Darker":
            bg = "#0a0a0a"
            frame_bg = "#121212"
            accent = "#bb86fc"
        else:  # Midnight
            bg = "#000033"
            frame_bg = "#0a0a1a"
            accent = "#ff9800"
        
        # Apply theme colors
        self.root.configure(bg=bg)
        self.style.configure(".", background=bg, foreground="#e0e0e0")
        self.style.configure("TFrame", background=frame_bg)
        self.style.configure("Header.TLabel", background=frame_bg, foreground=accent)
        self.style.configure("TButton", background="#333", foreground="#fff")
        self.style.configure("TCheckbutton", background=frame_bg, foreground="#e0e0e0")
        self.style.configure("TNotebook", background=bg)
        self.style.configure("TNotebook.Tab", background="#333", foreground="#fff")
        self.style.map("TNotebook.Tab", background=[("selected", accent)])
        
        # Update social button style
        self.style.configure("SocialBig.TButton", 
                             background=frame_bg, 
                             foreground=accent,
                             font=("Arial", 16, "bold"))

if __name__ == "__main__":
    root = tk.Tk()
    app = StorageDashboard(root)
    root.mainloop()
