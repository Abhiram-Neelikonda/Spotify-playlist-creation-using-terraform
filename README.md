# Spotify-playlist-creation-using-terraform
In this project I've created a spotify playlist using terraform via docker
To create a Spotify playlist using Terraform, you’ll need to set up the Spotify API provider in Terraform. Here’s a step-by-step process to guide you through it:

### Prerequisites

1. **Spotify API Key**: Create a Spotify Developer account and set up a new application. This will give you a `client_id` and `client_secret`, which are required to authenticate with Spotify.
2. **Terraform Installed**: Ensure Terraform is installed on your machine.

### Steps

#### Step 1: Set Up Spotify API Application

- Go to [Spotify Developer Dashboard](https://developer.spotify.com/dashboard/) and create an application.
- In the app settings, note down the `client_id` and `client_secret`.
- Add redirect URIs as needed for testing purposes.

#### Step 2: Install the Spotify Terraform Provider

You’ll use the Spotify provider for Terraform. Add it to your configuration file. 

#### Step 3: Create Terraform Configuration for Spotify

1. **Initialize the Provider**: In your Terraform configuration file (e.g., `main.tf`), define the Spotify provider, passing in your API credentials.

   ```hcl
   terraform {
     required_providers {
       spotify = {
         source = "conradludgate/spotify"
         version = "1.2.0" # Use the latest stable version
       }
     }
   }

   provider "spotify" {
     client_id     = var.spotify_client_id
     client_secret = var.spotify_client_secret
     redirect_uri  = "http://localhost:8888/callback"
   }
   ```

2. **Define Variables**: Create a `variables.tf` file to manage sensitive information like `client_id` and `client_secret`.

   ```hcl
   variable "spotify_client_id" {}
   variable "spotify_client_secret" {}
   ```

   In a `.tfvars` file (e.g., `secrets.tfvars`), define values for these variables.

   ```hcl
   spotify_client_id     = "your_client_id"
   spotify_client_secret = "your_client_secret"
   ```

3. **Create the Playlist Resource**: Add the resource for the playlist you want to create. You can customize the playlist name, description, and whether it’s public or private.

   ```hcl
   resource "spotify_playlist" "my_playlist" {
     name        = "My Terraform Playlist"
     description = "Created with Terraform"
     public      = true
   }
   ```

4. **Add Tracks to the Playlist**: Specify tracks you want to add by their Spotify IDs.

   ```hcl
   resource "spotify_playlist_track" "playlist_tracks" {
     playlist_id = spotify_playlist.my_playlist.id
     track_ids   = [
       "spotify_track_id_1",
       "spotify_track_id_2"
     ]
   }
   ```

   Replace `"spotify_track_id_1"` and `"spotify_track_id_2"` with actual track IDs from Spotify.

#### Step 4: Initialize and Apply Terraform

1. **Initialize Terraform**:

   ```bash
   terraform init
   ```

2. **Apply the Configuration**:

   ```bash
   terraform apply -var-file="secrets.tfvars"
   ```

   This will prompt you to authenticate with Spotify, and then create the playlist with the specified tracks.

#### Step 5: Verify the Playlist in Spotify

Once the configuration is applied successfully, check your Spotify account for the new playlist.

### Notes

- Make sure the permissions required for the Spotify API are set in the Spotify developer portal.
- Keep your `client_id` and `client_secret` secure, and consider using environment variables or a secrets management tool for production deployments.

This setup automates playlist creation on Spotify using Terraform, making it easy to reproduce and manage.
