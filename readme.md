# PS5 Streaming Without a Capture Card

This guide explains how to stream directly from your PS5 using Docker. It tricks the PS5 into thinking it's streaming to Twitch while redirecting the stream to your own PC.

---

## Step 1: Find Your Local IP Address

Before starting, you need to find the local IP of the computer running this setup.

### Windows:

1. Open Command Prompt and run:
   ```sh
   ipconfig
   ```
2. Find the "IPv4 Address" under the network you're connected to (Wi-Fi or Ethernet).

### macOS:

1. Open Terminal and run:
   ```sh
   ipconfig getifaddr en0
   ```
   _(For Ethernet, use `en1` instead of `en0`.)_

### Linux:

1. Open Terminal and run:
   ```sh
   hostname -I
   ```
2. The first IP in the list is usually your local IP.

Note down the IP address (e.g., `192.168.x.xxx`), as you will need it.

---

## Step 2: Start the Services

Ensure you have Docker installed, then navigate to the repository folder and run:

```sh
docker-compose -p playstation-rtmp up -d
```

This will start the necessary services in the background.

---

## Step 3: Set Up the PS5

1. **Sign into Twitch** on your PS5.
2. **Go to Settings > Network > Set Up Internet Connection**.
3. Choose your network (Wi-Fi or Ethernet) and select **Advanced Settings**.
4. Change **DNS Settings** to **Manual** and set **Primary DNS** to your computer's local IP (from Step 1).
5. Save the settings and restart your PS5.

---

## Step 4: Start Streaming

1. Start streaming on your PS5 as if you were streaming to Twitch.
2. Your stream will now be sent to your PC instead.

---

## Step 5: Add the Stream to OBS

1. Open a web browser and go to:
   ```
   http://<your-local-ip>:8081
   ```
   _(Replace `<your-local-ip>` with the IP you found in Step 1.)_
2. You will see a page showing the stream key. Copy the text that looks like:
   ```
   live_66666666_XXXXXX
   ```
3. Open OBS and add a **Media Source**.
4. Uncheck **Local File** and enter the following URL in the **Input** field:
   ```
   rtmp://<your-local-ip>/app/live_66666666_XXXXXX
   ```
   _(Replace `<your-local-ip>` with the IP you found in Step 1 and use the stream key you copied in Step 2.)_
5. Click **OK** and you should see your PS5 stream in OBS.

---

## Additional Notes

- Chat will not work.
- Live notifications (viewer count, alerts) will not work. (This is being worked on.)
- If you close the terminal after starting the stream, the service will keep running. To stop it manually, run:
  ```sh
  docker-compose -p playstation-rtmp down
  ```
- If you restart your computer, you can start streaming again by making sure the Docker container is running. Open Docker, go to **Containers/Apps**, find `playstation-rtmp`, and start it if it's not already running.
