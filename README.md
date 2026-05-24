# 🚀 The Complete Chronicle of AlmostNAS

> **"What is AlmostNAS?"** > In simple words, it is my own little home server, built out of desperation, powered by a 15-year-old laptop, and surviving on pure developer willpower. 

---

## 🧭 The "Why?" (The Storage Crisis)
One random day, my phone's storage hit the red zone. I had completely maxed out Google Cloud's free 15GB, and I refused to pay for a subscription. I had over 3TB of storage sitting on my gaming PC, so I asked an AI a simple question: *"How do I access the files on my PC from anywhere?"*

It gave me three paths:
1. Backup everything to a different cloud provider (Defeated the whole purpose).
2. Set up full remote desktop access (Too much of a hassle just to pull up a single image).
3. **Turn the PC into a NAS (Network Attached Storage).**

I wanted to keep using my PC for gaming, so wiping it to install a dedicated NAS OS wasn't an option. I briefly considered running a virtual machine (VM) inside Windows, but that meant leaving my power-hungry gaming rig running 24/7. 

Luckily, I remembered I had a really old, beaten-up laptop gathering dust in the closet. The server was born, and I named it **AlmostNAS** (a nod to my personal brand, *AlmostNawaal*).

### 💻 Hardware Specifications
* **CPU:** 2x Intel(R) Pentium(R) CPU B950 @ 2.10GHz
* **RAM:** 2GB
* **Storage:** 500GB HDD (A 15-year-old mechanical drive)

---

## 🛠️ Phase 1: The "Noob" Era & The Photo Crisis
The goal was simple: get the server running and upload my phone's photos to it.

I started by installing **OpenMediaVault (OMV)**. At the time, I had absolutely no idea that self-hosted applications existed. I was literally dragging and dropping photos directly into a bare network directory. It was painful, but to me, it felt like magic. 

Then, I discovered **Immich**. Having zero clues about how heavy Immich's AI face-recognition and thumbnail-generation features were, I deployed it. The laptop immediately began to choke, beg for mercy, and freeze. 

> 💡 **The Breakthrough:** Up until this point, I had no clue I could just SSH into the server from my PC to copy-paste commands. I was manually typing out every single docker and linux command on the laptop's keyboard. **Pure pain.**

I scrambled to find a lightweight alternative and landed on **Lychee**. It ran smoothly, but then I hit the true physical bottleneck: the 15-year-old hard drive. **It took an entire night to upload just 80 photos.** With a gallery of 5,000+ photos waiting, I realized this ancient hardware wasn't meant for media backup. Disheartened, I left the server dead and unplugged for a couple of months.

---

## 🎬 Phase 2: The "Arr" Stack & The Pi-hole Loop
One day, a YouTube video titled *"A NAS is more powerful than you think"* popped up on my feed. It opened my eyes to the world of local media streaming. I decided to give the laptop a second chance.

I started with **Plex**. At first, I was manually downloading movies on my main gaming PC and transferring them over the network to the server. 

Then, I saw someone use **Jellyseerr**. My jaw dropped. *This* was the automated paradise I was looking for. I spent an entire night diving down the rabbit hole, setting up a 6-to-7 container microservice ecosystem known as the **"Arr" Stack**:
* **Sonarr & Radarr** (Tv/Movie management)
* **QBittorrent** (Downloader running straight on the server)
* **Prowlarr** (Indexer)
* **Jellyseerr** (The beautiful request UI)

To access my new movie library on the go, I set up **Tailscale**—hands down the most beautiful, zero-config VPN service in existence.

### 🛑 The Reality Check
The automation worked perfectly, but the hardware limitations struck again. The old HDD bottlenecked torrent download speeds, and the ancient dual-core Pentium CPU didn't have the power to transcode video. If a file was anything above 720p, the stream would buffer indefinitely. 

### 🛡️ The Final Touch: Pi-hole Setup
Before wrapping up Phase 2, I decided to deploy **Pi-hole** on the server to block ads network-wide. I changed my home router's upstream DNS settings to point directly to the laptop's internal IP. It worked flawlessly for my phone and desktop, so I finally walked away happy.

---

## 🤖 Phase 3: The Microservice Graduation
After ditching media streaming, I shifted my focus to software development. Today, **AlmostNAS** doesn't really function like a traditional NAS anymore—it has been completely repurposed into a backend production environment hosting an **Age of Empires IV Discord Scout Bot**. 

Instead of heavy file transfers or video transcoding, this little machine masterfully handles a sleek microservices stack:
* A **Python fetching service** that polls the AOE4 World API.
* A **Redis container** for lightning-fast data caching.
* A **PostgreSQL database** for persistent player tracking.

 This phase also brought its own mountain of headaches. One of the biggest roadblocks was that **the containers suddenly stopped talking to each other**. Services that were supposed to be sharing data on the same network were timing out and failing. After a massive struggle to decode what was going wrong, the trail led right back to **Pi-hole**. Because it was acting as the DNS filter, it was aggressively blocking internal routing requests and generating massive DNS conflicts between the containers. Breaking through this required restructuring how the network bridge handled queries, turning it into a brutal debugging lesson on local infrastructure management.

---
## 📝 Future TODO List

- [ ] Upload the `docker-compose.yml` and all relevant environment configuration files to GitHub.
- [ ] Map out and document the complete system architecture for the entire home server.
- [ ] Implement a proxy manager properly to set up all the custom domain names and secure them with SSL.
- [ ] Scale the bot (optimize database queries, handle multi-server Discord growth, or break down the ingestion script into independent workers).
