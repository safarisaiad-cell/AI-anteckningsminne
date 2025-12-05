# security_ai.py
import pandas as pd
from sklearn.ensemble import IsolationForest

# 1. Läs in loggdata
# Exempel: CSV-fil med kolumner: timestamp, user, action, ip_address
log_data = pd.read_csv('system_logs.csv')

# 2. Förbehandling: konvertera IP-adresser och kategoriska data till numeriska
log_data['ip_numeric'] = log_data['ip_address'].apply(lambda x: int(''.join(x.split('.'))))
log_data['user_id'] = log_data['user'].astype('category').cat.codes
log_data['action_id'] = log_data['action'].astype('category').cat.codes

# 3. Skapa features för modellen
features = log_data[['user_id', 'action_id', 'ip_numeric']]

# 4. Träna Isolation Forest för att upptäcka anomalier
model = IsolationForest(contamination=0.01, random_state=42)  # 1% misstänkta aktiviteter
model.fit(features)

# 5. Gör prediktioner: -1 = anomal, 1 = normal
log_data['anomaly'] = model.predict(features)

# 6. Visa misstänkta aktiviteter
suspicious_logs = log_data[log_data['anomaly'] == -1]
print("Misstänkta aktiviteter:")
print(suspicious_logs[['timestamp', 'user', 'action', 'ip_address']])
