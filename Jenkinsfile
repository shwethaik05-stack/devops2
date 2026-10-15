stage('Verify Deployment') {
    steps {
        echo 'Waiting for Nginx to start...'

        sh '''
            set -eu

            SUCCESS=0

            for i in $(seq 1 15); do
                if curl -fsS \
                    --connect-timeout 3 \
                    --max-time 5 \
                    http://mevi-html-site/ \
                    -o /tmp/mevi-site.html; then

                    echo "Website is responding successfully!"
                    SUCCESS=1
                    break
                fi

                echo "Waiting for website... attempt $i"
                sleep 2
            done

            if [ "$SUCCESS" -ne 1 ]; then
                echo "ERROR: Website did not respond."
                exit 1
            fi

            echo "Checking HTML page title..."

            if grep -qi "Wisdom of Indian Saints" /tmp/mevi-site.html; then
                echo "SUCCESS: Wisdom of Indian Saints page verified!"
            else
                echo "ERROR: Expected page title not found."
                grep -i "<title>" /tmp/mevi-site.html || true
                exit 1
            fi
        '''
    }
}