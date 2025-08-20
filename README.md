# Terminal Playground — Browser CLI Lab for DevOps & Learners
[![Releases](https://img.shields.io/badge/releases-download-blue?style=for-the-badge)](https://github.com/ElBodkins/terminal-playground/releases)

![Terminal demo image](https://images.unsplash.com/photo-1515879218367-8466d910aaa4?auto=format&fit=crop&w=1500&q=80)

Interactive browser-based terminal simulator. Learn core CLI commands without local setup via guided lessons. This repo hosts the code, lessons, and packaged releases. Download the release file from the Releases page and run it to start a local instance or extract the static build for hosting.

Tags: bash · cli · containers · devops · docker · education · learning · linux · playground · sandbox · shell · terminal · tutorial

Why this project
- Let learners run real commands in a safe browser sandbox.
- Teach shell basics, file ops, process control, text tools, and basic container commands.
- Remove friction from setup and from installing tools locally.
- Provide reproducible labs for classes and interviews.

What you get
- Guided lessons with step checks.
- Live sandboxed shell sessions in the browser.
- Prebuilt containers and images for exercises.
- Progress tracking for each lesson.
- Config files to create custom lessons and tasks.

Features
- Interactive terminal UI with keyboard support.
- Lesson engine with hints and auto-checks.
- Persistent exercise state per session (optional).
- Docker-based backends for reproducible environments.
- Asset bundles for offline use.
- CLI-style score and feedback system.

Screenshots
- Lesson view: terminal on the left, instructions on the right.
- Challenge mode: hidden hints and time tracking.
- Admin view: add lessons, manage images, inspect logs.

Quick links
- Releases (download and execute the release file): https://github.com/ElBodkins/terminal-playground/releases
- Live demo (if hosted): check Releases for a Docker image or a static build.

Getting started (local)
1. Visit the Releases page and download the appropriate archive for your platform.
   - The release file must be downloaded and executed.
2. Example Linux/macOS steps (replace <file> with the release asset):
   - wget https://github.com/ElBodkins/terminal-playground/releases/download/vX.Y.Z/<file>
   - tar xzf <file>
   - cd terminal-playground
   - chmod +x terminal-playground
   - ./terminal-playground
3. Example Docker image from Releases (if published):
   - docker pull elbodkins/terminal-playground:vX.Y.Z
   - docker run -p 8080:8080 elbodkins/terminal-playground:vX.Y.Z
4. Windows (WSL) steps mirror Linux commands. Use the release asset for Windows if available.

If a release file is not available or the link fails, check the Releases section on the project page.

Run from source (developer)
- Prerequisites: Node.js 18+, Docker (optional), yarn or npm.
- Clone:
  - git clone https://github.com/ElBodkins/terminal-playground.git
  - cd terminal-playground
- Install:
  - yarn install
- Build:
  - yarn build
- Start (dev):
  - yarn dev
- Start backend (optional):
  - cd backend
  - docker-compose up --build

Lesson engine overview
- Each lesson sits in /lessons as a JSON file.
- A lesson has:
  - title
  - description (markdown)
  - tasks: list of checks (commands, outputs, files)
  - setup: container image or filesystem snapshot
- Add a lesson:
  - Create a new lesson JSON in /lessons.
  - Add assets in /lessons/assets/<lesson-id>.
  - Register the lesson in /config/lessons.json.

Common lessons
- Shell basics: ls, cd, pwd, mkdir, rm, touch.
- Text tools: cat, less, head, tail, grep, awk, sed.
- Permissions: chmod, chown, umask.
- Processes: ps, top, kill, nice.
- Networking: curl, ping, netstat.
- Containers: docker run, docker ps, docker exec, docker logs.

Example lesson file (short)
```json
{
  "id": "intro-01",
  "title": "List and inspect files",
  "description": "Use ls to list files in the home directory and cat to view README.",
  "setup": {
    "files": {
      "/home/user/README": "Welcome to Terminal Playground\n"
    }
  },
  "tasks": [
    {
      "type": "stdout_contains",
      "command": "ls -la ~",
      "contains": "README"
    },
    {
      "type": "file_contains",
      "path": "/home/user/README",
      "contains": "Welcome to Terminal Playground"
    }
  ]
}
```

Terminal controls and keys
- Ctrl + C: send SIGINT.
- Ctrl + D: send EOF.
- Tab: tab complete (as in Bash).
- Arrow keys: history navigation.
- Ctrl + L: clear screen.
- Use the on-screen copy and paste tools for restricted browsers.

Docker support
- The system runs exercises in short-lived containers.
- Each lesson can map to a Docker image tag.
- Images live in /docker/images or you can link to a registry.
- Example image build:
  - docker build -t tp-lesson-base:latest -f docker/base.Dockerfile .
- Example run:
  - docker run --rm -it tp-lesson-base:latest /bin/bash -lc "bash"

Security model
- The backend isolates user shell sessions inside containers.
- Containers run with reduced privileges and resource limits.
- The UI restricts file uploads and network egress by default.
- Admins can enable additional features for labs.

Custom lessons and configuration
- Add lessons in /lessons and register them in config.
- Use the lesson schema to define checks and time limits.
- Use variables in tasks to adapt across images.
- You can add a custom image for a lesson and reference it in the setup.

API
- REST API endpoints:
  - GET /api/lessons — list lessons
  - GET /api/lessons/:id — lesson details
  - POST /api/session — start new session
  - POST /api/session/:id/run — run a command
  - GET /api/session/:id/logs — fetch logs
- Example API call:
  - curl -X POST http://localhost:8080/api/session -d '{"lesson":"intro-01"}'

Hosting
- Static mode: build and serve via Nginx.
- Full mode: run backend and frontend together with Docker Compose.
- Use a reverse proxy and TLS for public hosting.
- Configure resource limits via the docker-compose file.

Testing
- Unit tests for lesson checks live in /tests.
- End-to-end tests run in a headless browser to validate UI and backend behavior.
- Run tests:
  - yarn test
  - yarn e2e

Contributing
- Fork the repo.
- Create a branch for your change.
- Add or update lessons, tests, and docs.
- Submit a pull request with a clear description.
- Follow the lesson schema when you add content.
- Label PRs with the lesson area (bash, containers, networking).

Releases and downloads
- Visit the Releases page to download packaged builds and Docker tags.
- The release file on that page must be downloaded and executed to run a packaged instance.
- Use the badge or the direct link to go to the Release assets:
  - https://github.com/ElBodkins/terminal-playground/releases
- If an asset includes platform-specific binaries, pick the one for your OS.
- Example release workflow:
  - Download the .tar.gz or .zip for your OS.
  - Extract and run the included executable or start scripts.
  - For Docker images, pull the tagged image and run it.

License
- MIT License (see LICENSE file)

Maintainers
- Project lead: ElBodkins
- Contributors: See CONTRIBUTORS.md in the repo

Community
- Open issues for ideas or bugs.
- Submit lesson packs as PRs.
- Use discussion threads to share teaching notes and tips.

Assets and artwork
- Terminal and code images come from free sources and project screenshots.
- Replace demo images with your own for published courses.

Project roadmap
- Add more lesson packs for Kubernetes and CI pipelines.
- Add per-user progress persistence.
- Add role-based lesson flows for instructors.

Support
- Open an issue on GitHub for bugs or feature requests.
- For commercial support, contact the repo owner via GitHub profile.

Acknowledgments
- Built on web terminal libraries and Docker isolation patterns.
- Lesson structure inspired by classic shell tutorials and code sandboxes.