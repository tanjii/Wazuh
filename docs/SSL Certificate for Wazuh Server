Provide certificates

Execute the following command to generate certificates:

```docker-compose -f generate-indexer-certs.yml run --rm generator```

Rename your SSL certificates:

cert.pem to wazuh.dashboard.pem

privateKey.pem to wazuh.dashboard-key.pem

Replace existing wazuh.dashboard.pem and wazuh.dashboard-key.pem certificates in the folder /home/wazuh-docker/multi-node/config/wazuh_indexer_ssl_certs with 3t.io certificates from step 1.

Updating certificates - just copy wazuh.dashboard.pem and wazuh.dashboard-key.pem and rename them as wazuh.dashboard<previous_expiration_year>.pem and wazuh.dashboard-key<previous_expiration_year>.pem (npr wazuh.dashboard26.pem) and change wazuh.dashboard.pem and wazuh.dashboard-key.pem content with the new certificate and it’s key. Then run 

docker compose down
docker compose up -d
