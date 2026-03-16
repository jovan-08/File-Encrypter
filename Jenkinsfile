node('agent') {
    try {

        stage('Build') {
            sh '''
            echo "Building Java project..."
            ls
            cd "Password Protection"
            mkdir -p build
            javac -d build src/*.java
            echo "Build successful"
            '''
        }

        stage('Test') {
            sh '''
            echo "Running JUnit tests..."
            cd "Password Protection"

            rm -f junit-platform-console-standalone.jar

            curl -L -o junit-platform-console-standalone.jar \
            https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar

            if [ -d "test" ]; then
                mkdir -p test-build
                javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java

                java -jar junit-platform-console-standalone.jar \
                --class-path build:test-build \
                --scan-class-path
            else
                echo "No tests found"
            fi
            '''
        }

        stage('Deploy') {
            sh '''
            echo "Packaging application..."
            cd "Password Protection"
            jar cf FileEncrypter.jar -C build .
            '''
        }

        echo "Pipeline executed successfully!"

    } catch (Exception e) {
        echo "Pipeline failed!"
        throw e
    }
}
