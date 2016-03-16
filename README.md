Getting Started
---------------
1. Spin up the template (heatdev.yaml), specifying the parameters
2. Let that finish and log into the box:
  ```
  heat stack-show <your stack name>
  ssh -A -o PubkeyAuthentication=no root@<server IP reported by stack show>
  <copy-paste server password reported by stack show>
  ```
  **Note:** This ssh command uses -A to forward the ssh agent, allowing you to pull private repositories to complete the install. This also means that you don't have to mess with private ssh keys in the template!

3. Install Heat, specifying what directory to install in:

  **Note:** You must specify an existing branch. The installer will not run heat-build. Instead, create a build using Jenkins.

  **Note:** Using a new directory installs fresh, while using an existing directory will be update the install in there.
  ```
  cd ~/heatdev
  ansible-playbook -i localhost, install_heat.yml --extra-vars "dir=heat_8000 branch=PRODUCTION port=8000"
  ```
  or alternately, install Fusion (or do both):
  ```
  cd ~/heatdev
  ansible-playbook -i localhost, install_fusion.yml --extra-vars "dir=fus8001_heat8000 branch=master port=8001 heat_port=8000"
  ```
  **Note:** The `heat_port` will point Fusion at `localhost:<heat_port>`, so it's probably best to install an instance of Heat on the box too.

4. Go into the directory you specified and start Heat:
  ```
  cd ~/heat_8000
  ./api_starter.sh & ./engine_starter.sh
  ```
  I recommend installing screen or tmux so that you can view each console separately!

5. Test it out
  ```
  curl localhost:8000
  # Responds with {"versions": [{"status": "CURRENT", "id": "v1.0",
  #                              "links": [{"href": "http://localhost:8007/v1/", "rel": "self"}]}]}#
  ```
