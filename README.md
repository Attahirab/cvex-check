import requests
import json

# Replace these with your actual wallet address and API keys
WALLET_ADDRESS = 0xf168202D5eaB78a7DA7b21f9cbf2A6F1C0Bb263d
GALXE_API_KEY = "YOUR_GALXE_API_KEY"  # Obtain from Galxe if required
LAYER3_API_KEY = "YOUR_LAYER3_API_KEY"  # Obtain from Layer3 if required
CVEX_API_KEY = "YOUR_CVEX_API_KEY"  # Obtain from CVEX if required

# Placeholder API endpoints (replace with actual endpoints from platform documentation)
CVEX_LEADERBOARD_API = "https://api.cvex.trade/leaderboard"  # Hypothetical
GALXE_CAMPAIGN_API = "https://api.galxe.com/v1/campaigns/CVEX/ranking"  # Hypothetical
LAYER3_QUEST_API = "https://api.layer3.xyz/quests/ranking"  # Hypothetical
CVEXTOPIA_API = "https://api.cvex.trade/cvextopia/ranking"  # Hypothetical (assuming CVEXtopia is a campaign)

# Headers for API requests (modify based on platform requirements)
HEADERS = {
    "Authorization": f"Bearer {GALXE_API_KEY}",  # Example for Galxe
    "Content-Type": "application/json"
}

def check_cvex_rank():
    """Check rank on CVEX project overall."""
    try:
        response = requests.get(
            CVEX_LEADERBOARD_API,
            headers={"Authorization": f"Bearer {CVEX_API_KEY}"},
            params={"wallet": 0xf168202D5eaB78a7DA7b21f9cbf2A6F1C0Bb263d}
        )
        response.raise_for_status()
        data = response.json()
        
        # Assuming response contains a list of users with ranks
        for user in data.get("leaderboard", []):
            if user.get("wallet").lower() == 0xf168202D5eaB78a7DA7b21f9cbf2A6F1C0Bb263d():
                print(f"CVEX Overall Rank: {user.get('rank')} (XP: {user.get('xp', 0)})")
                return
        print("CVEX Rank: Not found for your wallet address.")
    except requests.RequestException as e:
        print(f"Error checking CVEX rank: {e}")

def check_galxe_rank():
    """Check rank on Galxe for CVEX Testnet Expedition."""
    try:
        response = requests.get(
            GALXE_CAMPAIGN_API,
            headers=HEADERS,
            params={"wallet": 0xf168202D5eaB78a7DA7b21f9cbf2A6F1C0Bb263d}
        )
        response.raise_for_status()
        data = response.json()
        
        # Assuming response contains ranking info
        rank = data.get("rank")
        points = data.get("points", 0)
        if rank:
            print(f"Galxe CVEX Campaign Rank: {rank} (Points: {points})")
        else:
            print("Galxe Rank: Not found or campaign does not support ranking.")
    except requests.RequestException as e:
        print(f"Error checking Galxe rank: {e}")

def check_layer3_rank():
    """Check rank on Layer3 Quest Testnet."""
    try:
        response = requests.get(
            LAYER3_QUEST_API,
            headers={"Authorization": f"Bearer {LAYER3_API_KEY}"},
            params={"wallet": 0xf168202D5eaB78a7DA7b21f9cbf2A6F1C0Bb263d}
        )
        response.raise_for_status()
        data = response.json()
        
        # Assuming response contains user rank and XP
        rank = data.get("rank")
        xp = data.get("xp", 0)
        if rank:
            print(f"Layer3 Quest Testnet Rank: {rank} (XP: {xp})")
        else:
            print("Layer3 Rank: Not found or testnet does not support ranking.")
    except requests.RequestException as e:
        print(f"Error checking Layer3 rank: {e}")

def check_cvextopia_rank():
    """Check rank on CVEXtopia campaign (assumed to be part of CVEX)."""
    try:
        response = requests.get(
            CVEXTOPIA_API,
            headers={"Authorization": f"Bearer {CVEX_API_KEY}"},
            params={"wallet": WALLET_ADDRESS}
        )
        response.raise_for_status()
        data = response.json()
        
        # Assuming response contains ranking info
        rank = data.get("rank")
        points = data.get("points", 0)
        if rank:
            print(f"CVEXtopia Campaign Rank: {rank} (Points: {points})")
        else:
            print("CVEXtopia Rank: Not found or campaign does not exist.")
    except requests.RequestException as e:
        print(f"Error checking CVEXtopia rank: {e}")
