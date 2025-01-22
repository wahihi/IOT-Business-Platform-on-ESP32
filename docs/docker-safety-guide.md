# Docker Container Safety Guidelines for ESP32 Development

## Safe Container Management Practices

### 1. Always Use Volume Mounts
```bash
# Always include volume mounts when running container
docker run -it \
  --name ESP32_Project \
  --device=/dev/ttyUSB0 \
  -v $HOME/esp_projects:/root/esp \  # Mount working directory
  -v $HOME/esp_backup:/backup \      # Mount backup directory
  esp32_start
```

### 2. Regular Backup Procedures
```bash
# Backup at the end of each work session
tar -czf /backup/esp_backup_$(date +%Y%m%d).tar.gz /root/esp/*

# Or save container state after significant changes
docker commit ESP32_Project esp32_backup_$(date +%Y%m%d)
```

### 3. Document Your Work
```bash
# Keep a work log in backup directory
echo "$(date): [Work Description]" >> /backup/work_log.txt
```

### 4. Utilize GitHub
```bash
# Regularly push important code to GitHub
git add .
git commit -m "Describe your changes"
git push origin main
```

### 5. Create Container Launch Script
```bash
# run_container.sh
#!/bin/bash
docker run -it \
  --name ESP32_Project \
  --device=/dev/ttyUSB0 \
  -v $HOME/esp_projects:/root/esp \
  -v $HOME/esp_backup:/backup \
  esp32_start
```

### 6. Pre-Work Checklist
- [ ] Verify volume mounts
- [ ] Confirm backup directory exists
- [ ] Check Git repository connection
- [ ] Verify USB device connection

### 7. End-of-Work Checklist
- [ ] Backup critical files
- [ ] Commit and push to Git
- [ ] Record work log
- [ ] Save container state

## Best Practices for Data Safety

1. **Volume Management**
   - Never store important data only inside containers
   - Use named volumes for better management
   - Regularly check mount points

2. **Backup Strategy**
   - Implement automated backups
   - Keep multiple backup copies
   - Verify backup integrity regularly

3. **Version Control**
   - Commit changes frequently
   - Use meaningful commit messages
   - Maintain separate branches for experiments

4. **Container Management**
   - Use meaningful container names
   - Tag saved images with dates
   - Clean up unused containers regularly

5. **Development Environment**
   - Document environment setup
   - Use environment variables
   - Maintain setup scripts

## Recovery Procedures

### If Container Stops Unexpectedly
```bash
# Check container status
docker ps -a

# Restart container
docker start ESP32_Project

# Check logs
docker logs ESP32_Project
```

### If Data Loss Occurs
1. Check backup directory
2. Restore from latest backup
3. Check Git repository for latest commits
4. Review work logs

## Maintenance Schedule

- Daily: Commit code changes
- Weekly: Full backup
- Monthly: Environment review
- Quarterly: Cleanup unused images/containers

## Notes
- Always verify USB device connection before starting work
- Keep documentation up to date
- Test backup restoration periodically
- Maintain a list of critical files/directories