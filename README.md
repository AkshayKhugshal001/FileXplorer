FileXplore - Virtual File System Simulator
A complete virtual file system simulator implemented in C++17 that operates on a real folder sandbox. FileXplore provides both a command-line interface (CLI) and a modern web-based graphical interface (GUI) for managing files and directories.

🚀 Features
Core Functionality
Sandboxed Virtual File System: All operations are contained within a designated root directory
Unix-like Commands: Familiar command-line interface with standard file and directory operations
Path Safety: Prevents directory traversal attacks and access outside the sandbox
Persistent Storage: Works with real files and directories for data persistence
Dual Interface: Both CLI and web-based GUI modes available
CLI Mode Features
Interactive command-line interface
Command history (last 20 commands)
Real-time file system operations
Comprehensive error handling
GUI Mode Features
Modern, responsive web-based interface
Visual file system navigation
Drag-and-drop file upload
Multiple view modes (list, grid, tree)
Dark/light theme support
Real-time file system state visualization
Keyboard shortcuts and touch support
📋 Supported Commands
Directory Operations
mkdir <path> - Create directory
rmdir <path> - Remove empty directory
ls [path] - List directory contents
tree [path] - Display directory tree structure
cd <path> - Change current directory
pwd - Show current working directory
File Operations
create <path> - Create empty file
write <path> "content" - Write content to file (overwrite)
append <path> "content" - Append content to file
read <path> - Display file content
delete <path> - Delete file
System & Utility
df - Show disk usage statistics
history - Show last 20 commands
clear - Clear terminal screen
help - Display help information
exit - Exit FileXplore
Path Support
Absolute paths: /home/user/file.txt
Relative paths: documents/file.txt
Current directory: ./file.txt or file.txt
Parent directory: ../file.txt
🛠️ Building the Project
Prerequisites
For Windows (MSYS2/MinGW-w64)
MSYS2 installed (download from https://www.msys2.org/)
MinGW-w64 toolchain (GCC 9.0 or later for GUI support)
CMake 3.10 or later
Required libraries (install via MSYS2 pacman)
For Linux
C++17 compatible compiler (GCC 7+, Clang 5+)
CMake 3.10+
Make
pthread library
For macOS
Xcode Command Line Tools
CMake 3.10+
Make
MSYS2 Installation and Setup
If you're building on Windows with MSYS2, follow these steps:

Install MSYS2 (if not already installed)

Download from https://www.msys2.org/
Run the installer and follow the setup wizard
Open MSYS2 MinGW 64-bit terminal and update the package database:

pacman -Syu
Install required development tools:

pacman -S mingw-w64-x86_64-gcc
pacman -S mingw-w64-x86_64-cmake
pacman -S mingw-w64-x86_64-make
pacman -S git
Install required libraries for GUI support:

pacman -S mingw-w64-x86_64-asio
Verify installation:

gcc --version    # Should show GCC 9.0 or later
cmake --version  # Should show CMake 3.10 or later
Clone or navigate to the FileXplore project directory:

cd /c/path/to/FileXplore
Build Instructions
Option 1: Using CMake (Recommended)
Create build directory:

mkdir build
cd build
Configure the project:

cmake ..
For MSYS2/MinGW-w64, you may need to specify the generator:

cmake -G "MinGW Makefiles" ..
Build the project:

cmake --build .
Or using make (on Unix-like systems):

make
The executable will be in:

build/bin/FileXplore.exe  (Windows)
build/bin/FileXplore      (Linux/macOS)
Option 2: Using Makefile
make all          # Build both CLI and GUI
make cli          # Build CLI only (if Makefile supports it)
make gui          # Build GUI only (if Makefile supports it)
make clean        # Clean build files
Option 3: Manual Compilation
For CLI only:

g++ -std=c++17 -Wall -Wextra -Iinclude src/PathUtils.cpp src/FileManager.cpp src/DirManager.cpp src/CommandParser.cpp src/PersistenceManager.cpp src/HistoryManager.cpp src/SystemInfo.cpp main.cpp -o FileXplore
For GUI (requires Crow and nlohmann::json):

g++ -std=c++17 -Wall -Wextra -pthread -Iinclude -Ithird_party/include -Ithird_party/Crow-master/include -DASIO_STANDALONE -DCROW_STATIC_DIRECTORY="./web" src/*.cpp main.cpp -o FileXplore -lws2_32 -lmswsock
Build Configuration
The project uses CMake with the following options:

ENABLE_GUI: Enable/disable GUI support (default: ON)
C++17 Standard: Required for both CLI and GUI
Crow Web Framework: Required for GUI mode (header-only, included in third_party/)
nlohmann::json: Required for GUI mode (header-only, included in third_party/include/)
Asio: Required for GUI mode (standalone version, install via MSYS2)
🎮 Usage
Starting FileXplore
CLI Mode
# Use default VFS root (./filexplore_root)
./FileXplore

# Use custom VFS root directory
./FileXplore /path/to/custom/root
GUI Mode
# Start GUI mode with default VFS root
./FileXplore --gui

# Start GUI mode with custom VFS root
./FileXplore --gui /path/to/custom/root

# Alternative short flag
./FileXplore -g
Once the GUI server starts, open your web browser and navigate to:

http://localhost:8080
Example CLI Session
FileXplore:/$ mkdir /home
Directory created: /home

FileXplore:/$ cd /home
Changed directory to: /home

FileXplore:/home$ create welcome.txt
File created: welcome.txt

FileXplore:/home$ write welcome.txt "Hello, FileXplore!"
Content written to file: welcome.txt

FileXplore:/home$ read welcome.txt
Content of welcome.txt:
--------------------------------------------------
Hello, FileXplore!
--------------------------------------------------

FileXplore:/home$ ls
Contents of .:
  welcome.txt

FileXplore:/home$ tree /
/
└── home/
    └── welcome.txt

FileXplore:/home$ df
======================================================================
FileXplore Virtual File System Statistics
======================================================================
VFS Root:           /path/to/filexplore_root
Current Directory:  /home
----------------------------------------------------------------------
Total Files:        1
Total Directories:  1
Total Size:         17.00 B (17 bytes)
======================================================================
Demo Commands
A comprehensive demo commands file is included (demo_commands.txt). You can copy and paste commands from this file to test all features.

🏗️ Architecture
Project Structure
FileXplore/
├── include/                 # Header files
│   ├── PathUtils.h         # Path resolution and sandbox security
│   ├── FileManager.h       # File operations
│   ├── DirManager.h        # Directory operations
│   ├── CommandParser.h     # CLI command parsing
│   ├── HistoryManager.h    # Command history management
│   ├── SystemInfo.h        # System statistics
│   ├── PersistenceManager.h # State persistence
│   └── WebServer.h         # Web server for GUI mode
├── src/                     # Source files
│   ├── PathUtils.cpp
│   ├── FileManager.cpp
│   ├── DirManager.cpp
│   ├── CommandParser.cpp
│   ├── HistoryManager.cpp
│   ├── SystemInfo.cpp
│   ├── PersistenceManager.cpp
│   └── WebServer.cpp       # GUI web server implementation
├── web/                     # Frontend files (GUI)
│   ├── index.html          # Main HTML structure
│   ├── styles.css          # CSS styling
│   ├── app.js              # JavaScript application
│   └── test-server.py      # Python test server for frontend development
├── third_party/             # Third-party dependencies
│   ├── Crow-master/        # Crow web framework
│   └── include/
│       ├── crow/           # Crow headers
│       └── nlohmann/       # nlohmann::json header
├── filexplore_root/         # Default VFS root directory
├── main.cpp                 # Application entry point
├── CMakeLists.txt          # CMake build configuration
├── Makefile                # Alternative build system
├── demo_commands.txt       # Demo commands showcase
└── README.md               # This file
Class Structure
Core Components
PathUtils: Safe path resolution and sandbox security
FileManager: File operations (create, read, write, append, delete)
DirManager: Directory operations (mkdir, rmdir, ls, tree, cd, pwd)
CommandParser: CLI command parsing and execution
HistoryManager: Command history management (last 20 commands)
SystemInfo: System statistics and disk usage information
PersistenceManager: Session state persistence
GUI Components
WebServer: HTTP server using Crow web framework
RESTful API endpoints for all CLI commands
Static file serving for frontend
JSON response formatting
CORS support for local development
Security Features
Sandbox Enforcement: All paths are validated to prevent access outside VFS root
Path Normalization: Resolves .., ., and other path components safely
Input Validation: Comprehensive validation of all user inputs
Error Handling: Graceful handling of filesystem errors and edge cases
CORS Configuration: Restricts access to localhost by default in GUI mode
🌐 GUI API Endpoints
The GUI communicates with the backend through RESTful API endpoints:

GET /api/filesystem - Get current directory structure
POST /api/command - Execute CLI commands
GET /api/file/{path} - Download file content
POST /api/file/{path} - Upload/write file content
GET /api/history - Get command history
GET /api/system - Get system information
API Response Format
All API endpoints return JSON responses in the following format:

{
  "success": true,
  "message": "Operation message",
  "data": { ... }
}
🔧 Technical Details
Requirements
Language: C++17
Standard Library: Uses STL and std::filesystem for cross-platform compatibility
GUI Dependencies:
Crow web framework (header-only, included)
nlohmann::json (header-only, included)
Asio (standalone, requires installation via MSYS2 on Windows)
Compiler Support:
GCC 7+ (CLI), GCC 9+ (GUI)
Clang 5+ (CLI), Clang 7+ (GUI)
MSVC 2017+ (both CLI and GUI)
Key Features
Cross-platform: Works on Windows, Linux, and macOS
Memory Safe: Uses RAII and smart resource management
Exception Safe: Proper exception handling throughout
Performance: Efficient file operations with minimal overhead
🐛 Troubleshooting
Build Issues
MSYS2/MinGW-w64 Issues
Problem: CMake can't find Asio header

Solution: Ensure Asio is installed: pacman -S mingw-w64-x86_64-asio
Verify installation: Check that C:/msys64/mingw64/include/asio.hpp exists
Problem: GCC version too old for GUI

Solution: Update GCC: pacman -S mingw-w64-x86_64-gcc
GUI requires GCC 9.0 or later for full C++17 <string_view> support
Problem: CMake not found

Solution: Install CMake: pacman -S mingw-w64-x86_64-cmake
General Build Issues
Problem: C++17 features not recognized

Solution: Ensure compiler supports C++17. Check with g++ --version or clang++ --version
Update CMakeLists.txt if needed to explicitly set C++17 standard
Problem: Crow framework not found

Solution: Ensure third_party/Crow-master/include directory exists
The Crow framework should be included in the repository
Problem: nlohmann::json not found

Solution: Ensure third_party/include/nlohmann/json.hpp exists
The JSON library should be included in the repository
Runtime Issues
CLI Mode
Problem: Permission denied errors

Solution: Check file permissions on VFS root directory
Ensure the application has read/write access to the VFS root
Problem: Path traversal errors

Solution: This is expected behavior - FileXplore prevents access outside the sandbox
Use paths relative to the VFS root
GUI Mode
Problem: Port 8080 already in use

Solution: Stop the application using port 8080, or modify the port in main.cpp
Check with: netstat -ano | findstr :8080 (Windows) or lsof -i :8080 (Linux/macOS)
Problem: Web interface not loading

Solution:
Verify the server started successfully (check console output)
Ensure JavaScript is enabled in your browser
Check browser console for errors (F12)
Verify web/ directory contains index.html, styles.css, and app.js
Problem: API requests failing

Solution:
Check that the backend server is running
Verify CORS headers are being sent (check browser Network tab)
Ensure the VFS root directory is accessible
Development Tips
Use the Python test server (web/test-server.py) for frontend development without rebuilding
Test with different screen sizes for responsive design verification
Check browser console for JavaScript errors
Use browser developer tools for debugging API requests
Enable verbose logging in the backend for debugging
📝 Testing the Frontend
A Python test server is provided for testing the frontend without the C++ backend:

cd web
python3 test-server.py [port]
The test server runs on http://localhost:8080 by default and provides mock data for testing the GUI interface.

🤝 Contributing
Fork the repository
Create a feature branch
Make your changes
Add tests if applicable
Submit a pull request
📄 License
This project is open source and available under the MIT License.

🙏 Acknowledgments
Built with modern C++17 features
Inspired by Unix/Linux filesystem commands
Designed for educational and practical use
Uses Crow web framework for GUI mode
Uses nlohmann::json for JSON handling
FileXplore - Experience the power of virtual file systems! 🚀
