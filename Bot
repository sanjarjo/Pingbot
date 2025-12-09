import os
import asyncio
import aiohttp
from aiogram import Bot

# Load environment variables
TOKEN = os.getenv("BOT_TOKEN")
ADMIN_ID = int(os.getenv("ADMIN_ID"))
PING_URL_1 = os.getenv("PING_URL_1")
PING_URL_2 = os.getenv("PING_URL_2")
PING_INTERVAL = int(os.getenv("PING_INTERVAL", 270))

bot = Bot(token=TOKEN)


async def send_status(ok: bool, url: str, description: str):
    """Send formatted status message to admin."""
    if ok:
        text = f"""
STATUS: True
Pinged: {url}
Response: {description}
        """.strip()
    else:
        text = f"""
STATUS: False
Pinged: {url}
Description: {description}
        """.strip()

    await bot.send_message(ADMIN_ID, text)


async def ping_url(url: str):
    """Ping the URL and return result"""
    try:
        async with aiohttp.ClientSession() as session:
            async with session.get(url, timeout=6) as resp:
                status = resp.status
                return True, f"{status} OK"
    except Exception as e:
        return False, str(e)


async def main_loop():
    await bot.send_message(ADMIN_ID, "🚀 Ping bot ishga tushdi!")

    while True:
        # Ping 1: self ping
        ok1, desc1 = await ping_url(PING_URL_1)
        await send_status(ok1, PING_URL_1, desc1)

        # Ping 2: second URL
        ok2, desc2 = await ping_url(PING_URL_2)
        await send_status(ok2, PING_URL_2, desc2)

        await asyncio.sleep(PING_INTERVAL)


if __name__ == "__main__":
    asyncio.run(main_loop())
