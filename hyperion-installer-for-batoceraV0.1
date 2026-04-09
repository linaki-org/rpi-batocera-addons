#!/bin/bash
#
# Installs Hyperion on a Raspberry Pi running Batocera
set -e

# Install directory and log file
dir_install="/userdata/system/hyperion"
log_file="/userdata/system/logs/hyperion_install.log"
echo "Let's install Hyperion !"

echo "Deleting old Hyperion installs..." | tee "$log_file"
rm -rf "$dir_install"
mkdir -p "$dir_install"
echo "Done !"

# Get last ARM64 release of Hyperion on GitHub
echo "Getting latest release URL..." | tee -a "$log_file"
latest_url=$(wget -qO- https://api.github.com/repos/hyperion-project/hyperion.ng/releases/latest | sed -nE 's/.*"browser_download_url": "([^"]*Linux-arm64\.tar\.gz)".*/\1/p')
if [ -z "$latest_url" ]; then
    err "Cannot get Hyperion latest release. Check your internet connection and try again." | tee -a "$log_file"
    exit 1
fi
echo "Done !" | tee -a "$log_file"

echo "Downloading latest release..." | tee -a "$log_file"
wget -q --show-progress -O "$dir_install/hyperion.tar.gz" "$latest_url"

echo "Extracting Hyperion..." | tee -a "$log_file"
tar -xzf "$dir_install/hyperion.tar.gz" -C "$dir_install" --strip-components=1
rm "$dir_install/hyperion.tar.gz"

# Adjusting binary permissions
chmod +x "$dir_install/hyperion/bin/hyperiond"

# Creating the Hyperion Batocera service
service_path="/userdata/system/services/hyperion"
cat > "$service_path" << EOF
#!/bin/bash

HYPERION_PATH="$dir_install/hyperion/bin/hyperiond"
LOG_FILE="/userdata/system/logs/hyperion.log"

case "\$1" in
    start)
        if [ -x "\$HYPERION_PATH" ]; then
            echo "Starting Hyperion service..." | tee -a "\$LOG_FILE"
            "\$HYPERION_PATH" > "\$LOG_FILE" 2>&1 &
            echo \$! > /var/run/hyperion.pid
        else
            err "Cannot find Hyperion in \$HYPERION_PATH. Please check your install" | tee -a "\$LOG_FILE"
        fi
        ;;
    stop)
        echo "Stopping Hyperion service..." | tee -a "\$LOG_FILE"
        if [ -f /var/run/hyperion.pid ]; then
            kill -9 \$(cat /var/run/hyperion.pid) && rm -f /var/run/hyperion.pid
        else
            killall -9 hyperiond
        fi
        ;;
    restart)
        echo "Restarting Hyperion service..." | tee -a "\$LOG_FILE"
        \$0 stop
        sleep 1
        \$0 start
        ;;
esac
EOF

chmod +x "$service_path"


echo "Installing libsystemd.so..." | tee -a "\$LOG_FILE"
echo "Downloading package from debian..." | tee -a "\$LOG_FILE"
# Download the Debian arm64 libsystemd package
wget -O /tmp/libsystemd.deb "http://ftp.debian.org/debian/pool/main/s/systemd/libsystemd0_260.1-1_arm64.deb"

# Extract it
echo "Extracting libsystemd.deb..." | tee -a "\$LOG_FILE"
cd /tmp
ar x libsystemd.deb
tar -xf data.tar.* ./lib/aarch64-linux-gnu/libsystemd.so.0 ./lib/aarch64-linux-gnu/libsystemd.so.0.38.0 2>/dev/null || \
tar -xf data.tar.*

# Find the extracted .so
find /tmp -name "libsystemd.so*"

# Copy to system lib
echo "Installing system library..." | tee -a "\$LOG_FILE"
cp /tmp/usr/lib/aarch64-linux-gnu/libsystemd.so.0* /usr/lib/
ldconfig

echo "Saving batocera overlay..." | tee -a "\$LOG_FILE"
batocera-save-overlay

echo "Hyperion has successfully been installed on your Batocera install ! It is now available in your Batocera Services." | tee -a "$log_file"
