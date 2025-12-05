import re
from collections import Counter

# Exempel på loggar
loggar = [
    "2025-12-03 10:01:01 - INFO - User login: alice",
    "2025-12-03 10:02:15 - ERROR - Failed login: bob",
    "2025-12-03 10:03:20 - WARNING - Multiple failed logins: charlie",
    "2025-12-03 10:04:45 - INFO - User login: alice",
    "2025-12-03 10:05:00 - ERROR - Failed login: bob",
    "2025-12-03 10:06:30 - ERROR - Failed login: bob"
]

# Identifiera misstänkta aktiviteter (t.ex. fler än 2 misslyckade inloggningar)
mönster = re.compile(r"Failed login: (\w+)")
misslyckanden = [match.group(1) for logg in loggar for match in [mönster.search(logg)] if match]

räkna_misslyckanden = Counter(misslyckanden)

# Flagga misstänkta användare
för var användare, antal i räkna_misslyckanden.items():
    if antal > 2:
        print(f"⚠️ Misstänkt aktivitet upptäckt: {användare} har {antal} misslyckade inloggningar")
