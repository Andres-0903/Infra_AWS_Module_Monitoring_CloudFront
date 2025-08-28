📊 Módulo de Monitoreo para CloudFront en AWS
Este módulo de Terraform permite configurar alertas de monitoreo en Amazon CloudWatch para de CloudFront. Las alertas supervisan errores del servidor, latencia de origen y errores en funciones Lambda@Edge, permitiendo detección temprana de problemas y acción preventiva.

✅ Requisitos
Herramienta Versión mínima
Terraform >= 1.0
AWS Provider ~> 5.0
Random Provider ~> 3.4.3
📁 Archivos principales
main.tf: Configuración de recursos de CloudWatch (alarmas).
output.tf: Variables de salida del módulo.
variables.tf: Variables para personalización (nombres, umbrales, etc).
🔧 Configuración detallada
Alarmas configuradas:
5xxErrorRate – Porcentaje de errores 5xx de servidor:
resource "aws_cloudwatch_metric_alarm" "cloudfront_5xx_error_rate" {
...
}
OriginLatency – Tiempo de respuesta desde el origen:
resource "aws_cloudwatch_metric_alarm" "cloudfront_origin_latency" {
...
}
FunctionExecutionErrors – Errores en ejecución de funciones Lambda@Edge:
resource "aws_cloudwatch_metric_alarm" "cloudfront_function_execution_errors" {
...
}
Los threshold vienen por defecto y estos valores son los que se encuentran en el documento de lineamientos de monitoreo.

⚙️ Parámetros de las variables threshold
Variable Valor
cloudfront_5xx_error_rate_evaluation_periods 1
cloudfront_5xx_error_rate_period 300 segundos
cloudfront_5xx_error_rate_threshold 10 (10%)
cloudfront_origin_latency_evaluation_periods 1
cloudfront_origin_latency_period 300 segundos
cloudfront_origin_latency_threshold 80
cloudfront_function_execution_errors_evaluation_periods 1
cloudfront_function_execution_errors_period 300 segundos
cloudfront_function_execution_errors_threshold 0
⚙️ Parámetros configurables
Parámetro Descripción
var.service Nombre del servicio.
alarm_name Nombre de la alarma.
comparison_operator Operador de comparación (e.g., GreaterThanThreshold).
evaluation_periods Cantidad de períodos de evaluación antes de activar.
metric_name Nombre de la métrica monitoreada.
namespace Espacio de nombres de la métrica.
period Frecuencia de evaluación en segundos.
statistic Estadística usada (Average, Sum, etc).
threshold Valor límite para activar la alerta.
tags Etiquetas del banco: tag_environment, tag_app, etc.
🧪 Modo de uso
module "cloudfront_monitoring" {
source = "git::https://github.com/Andres-0903/Infra_AWS_Module_Monitoring_CloudFront.git//cloudfront?ref=main"
cloudfront_distribution = var.cloudfront_distribution
project = var.project
bdo_name_service = var.bdo_name_service
bdo_environment = var.bdo_environment
purpose = var.purpose

sns_topic_arn = var.sns_topic_arn
}
🧪 A tener en cuenta
Una vez creado el SNS Topic se debe dejar encryptado con su respectivo KMS

📝 Resource
https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cloudfront-metrics.html
Infra_AWS_Module_Monitoring_CloudFront
