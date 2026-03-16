node('agent') {

    try {

        stage('Initialization') {
            echo "==============================================="
            echo " Jenkins Distributed Build Pipeline Started"
            echo " Project  : File-Encrypter (Java AES Encryption Tool)"
            echo " Author   : Jovan Fernandes"
            echo " Node     : ${env.NODE_NAME}"
            echo " Workspace: ${env.WORKSPACE}"
            echo "==============================================="
        }

        stage('Build') {
            sh '''
            echo "-----------------------------------------------"
            echo "[BUILD STAGE]"
            echo "Compiling Java source files..."
            echo "Current workspace contents:"
            ls -la

            echo "Navigating to project directory..."
            cd "Password Protection"

            echo "Creating build directory..."
            mkdir -p build

            echo "Compiling source code using javac..."
            javac -d build src/*.java

            echo "Java compilation completed successfully."
            echo "-----------------------------------------------"
            '''
        }

        stage('Test') {
            sh '''
            echo "-----------------------------------------------"
            echo "[TEST STAGE]"
            echo "Preparing environment for JUnit tests..."

            cd "Password Protection"

            echo "Cleaning old JUnit artifacts if present..."
            rm -f junit-platform-console-standalone.jar

            echo "Downloading JUnit platform console..."
            curl -L -o junit-platform-console-standalone.jar \
            https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

            echo "Checking for test directory..."

            if [ -d "test" ]; then
                echo "Test directory found."
                echo "Compiling test classes..."

                mkdir -p test-build
                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

                echo "Executing JUnit tests..."

                java -jar junit-platform-console-standalone.jar \
                --class-path build:test-build \
                --scan-class-path

                echo "JUnit tests executed successfully."
            else
                echo "No test directory found. Skipping tests."
            fi

            echo "-----------------------------------------------"
            '''
        }

        stage('Deploy') {
            sh '''
            echo "-----------------------------------------------"
            echo "[DEPLOY / PACKAGE STAGE]"
            echo "Packaging compiled classes into executable JAR..."

            cd "Password Protection"

            jar cf FileEncrypter.jar -C build .

            echo "JAR package created successfully:"
            ls -lh FileEncrypter.jar

            echo "Deployment artifact ready."
            echo "-----------------------------------------------"
            '''
        }

        echo "==============================================="
        echo " Pipeline completed successfully!"
        echo " Executed by : Jovan Fernandes"
        echo " Node used   : ${env.NODE_NAME}"
        echo "==============================================="

    } catch (Exception e) {

        echo "==============================================="
        echo " Pipeline failed!"
        echo " Error occurred during execution."
        echo " Executed by : Jovan Fernandes"
        echo "==============================================="

        throw e
    }
}
