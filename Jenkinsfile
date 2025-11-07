pipeline{
    agent any
    
    stages{
        //parar todos los servicios 
        stage('Parando los servicios'){
            steps{
                sh '''
                    docker-compose -p adj-demo down || true
                '''
            }
        }

        //eliminar las imagenes anteriores
        stage('Borrando imagenes antiguas'){
            steps{
                sh '''
                    IMAGES=$(docker images --filter "label=com.docker.compose.project=adj-demo" -q)
                    if [ -n '$IMAGES' ]; then
                        docker images rmi $IMAGES
                    else
                        echos echo 'No hay imagenes para borrar'
                    fi
                '''
                
            }
        }

        //bajar la actualizacion
        stage('Actualizando....'){
            steps{
                checkout scm
            }
        }

        //levantar y desplegar el proyecto
        stage('Construyendo y Desplegando...'){
            steps{
                sh '''
                    docker compose up --build -d
                '''
            }
        }
    }

    post{
        success{
            echo 'Pipeline ejecutado exitosamente'
        }

        failure{
            echo 'Error al ejecutar el pipeline'
        }

        always{
            echo 'Pipeline finalizado'
        }
    }
}