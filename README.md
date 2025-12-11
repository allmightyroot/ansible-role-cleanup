# Ansible Role: cleanup

[![CI](https://github.com/allmightyroot/ansible-role-cleanup/workflows/CI/badge.svg)](https://github.com/allmightyroot/ansible-role-cleanup/actions)

## Description

Prepares cloud images for AMI creation by performing final cleanup operations. This role removes sensitive data, resets instance identifiers, and clears logs to ensure clean and unique instances are created from the image.

**Key responsibilities:**
- Remove default cloud provider users (ubuntu, ec2-user, etc.)
- Clear system logs
- Reset machine ID for unique instance identification
- Clear shell history
- Clean temporary files
- Set AMI metadata

## Requirements

- Ansible >= 2.9
- `community.general` collection >= 5.0.0

## Important Security Note

This role removes default cloud provider users. **Do not run this role while logged in as a user being removed** or you will be locked out. Ensure you are connected as root or a non-cloud user.

## Role Variables

All variables are prefixed with `cleanup_` and can be found in `defaults/main.yml`:

- `cleanup_cloud_users` - List of default cloud users to remove (ubuntu, ec2-user, etc.)
- `cleanup_ami_version: "1.0.0"` - AMI version string for metadata
- `cleanup_reset_machine_id: true` - Reset /etc/machine-id for unique instances
- `cleanup_clear_bash_history: true` - Clear bash history for all users
- `cleanup_clear_logs: true` - Truncate all log files in /var/log

## Dependencies

None.

## Example Playbook

```yaml
---
- name: Cleanup for AMI creation
  hosts: all
  become: yes

  roles:
    - role: cleanup
      vars:
        cleanup_ami_version: "1.0.0"
        cleanup_reset_machine_id: true
        cleanup_clear_bash_history: true
```

## Typical Usage Pattern

This role is typically used as the final step in an AMI build pipeline:

```yaml
roles:
  - role: baseline
  - role: hardening
  - role: ansible-user
  - role: cleanup        # Always run last
```

## Testing

To test this role locally:

```bash
ansible-playbook tests/test.yml -i tests/inventory
```

To test with Molecule (if configured):

```bash
molecule test
```

## License

MIT-0

## Author Information

Created by allmightyroot
