### Step-by-Step Refactoring

1. **Create a New Role:** Use the `ansible-galaxy` command to create a new role named `apache_install`:

   ```bash
   ansible-galaxy role init apache_install
   ```

   This will create a directory structure for your role `apache_install` with necessary subdirectories.

2. **Move Tasks to Role:** Open the newly created role directory and navigate to `tasks/main.yml`:

   ```bash
   cd apache_install/tasks
   ```

   Replace the contents of `main.yml` with the tasks from your original playbook:

   ```yaml
   ---
   - name: Install apache httpd
     ansible.builtin.apt:
       name: apache2
       state: present
       update_cache: yes

   - name: Copy file with owner and permissions
     ansible.builtin.copy:
       src: index.html
       dest: /var/www/html
       owner: root
       group: root
       mode: '0644'
   ```

   Save the changes to `main.yml`.

3. **Update Playbook to Use the Role:** Modify your playbook (`playbook.yml`) to include the `apache_install` role:

   ```yaml
   ---
   - hosts: all
     become: true
     roles:
       - apache_install
   ```

   Replace the entire contents of your original playbook with the above `hosts`, `become`, and `roles` directives.

4. **Move index.html to Role Files:** Assuming `index.html` is in your current directory, move it to the `files` directory within your role:

   ```bash
   mv index.html apache_install/files/index.html
   ```

5. **Update Task to Reference index.html in Role:** In `apache_install/tasks/main.yml`, ensure the `copy` task references `files/index.html`:

   ```yaml
   - name: Copy file with owner and permissions
     ansible.builtin.copy:
       src: files/index.html
       dest: /var/www/html
       owner: root
       group: root
       mode: '0644'
   ```

   Update `src: index.html` to `src: files/index.html` in the `copy` task.
