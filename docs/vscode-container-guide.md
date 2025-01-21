# Accessing Docker Containers with Visual Studio Code

## Prerequisites
- Visual Studio Code installed
- Docker running on your system
- Remote-Containers extension installed in VS Code

## Opening Container Folders

### Method 1: Using Command Line
```bash
# Navigate to your container directory
code .
```

### Method 2: Using VS Code Interface
1. Open Command Palette:
   - Windows/Linux: `Ctrl + Shift + P`
   - macOS: `Cmd + Shift + P`
2. Type and select "Remote-Containers: Attach to Running Container"
3. Select your container from the list (e.g., bc2480bd8a1a)
4. Navigate to "File > Open Folder" and select your desired directory

## Installing Remote-Containers Extension
If not already installed:
1. Open VS Code Extensions panel (Ctrl+Shift+X)
2. Search for "Remote-Containers"
3. Click "Install"

## Closing Container Connection

### Method 1: Using Menu
1. Navigate to File > Close Remote Connection
2. VS Code will return to local mode

### Method 2: Using Status Bar
1. Click on "Dev Container" or container name in bottom-left corner
2. Select "Close Remote Connection"

### Closing VS Code Window
After closing the container connection:
- Windows/Linux: `Alt + F4` or click X button
- macOS: `Cmd + Q` or click red close button

## Troubleshooting Tips
- Ensure Docker is running before connecting
- Check container status using `docker ps`
- Verify Remote-Containers extension is properly installed
- Restart VS Code if connection issues persist

## Notes
- Closing the container properly prevents potential file sync issues
- Remote connection status is always visible in bottom-left corner
- Container remains running after disconnecting VS Code
- Multiple VS Code windows can connect to different containers simultaneously