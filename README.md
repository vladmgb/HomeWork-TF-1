# Домашнее задание к занятию «Введение в Terraform»

## Чек-лист готовности к домашнему заданию
1. Cкриншот вывода команды `terraform --version`
![1](https://github.com/user-attachments/assets/22804ce8-620a-4e52-9770-908391586dc3)
3. docker установлен
![2](https://github.com/user-attachments/assets/33f920c8-a83a-4574-bb85-273219903390)

## Задание 1
2. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?  
   personal.auto.tfvars
4. Cекретное содержимое созданного ресурса random_password:  `"result": "iPgGDgMyrgZUch9V"`
5. Проверка `terraform validate`

   Разбор ошибок:
   - В коде было пропущено имя для ресурса "docker_image".
   - У ресурс nginx имя начиналось с цифры - "1nginx". Имя должно начинаться с буквы или подчеркивания.
   - К ресурсу "random_password" было неправильное обращение по имени "random_string_FAKE", а должно быть "random_string".
5. Исправленный фрагмент кода:
```HCL
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"
```
![3](https://github.com/user-attachments/assets/8c874eec-47c2-47cf-8fdb-52d3a52e4d78)

6. Ключ `-auto-approve`

    Опасность ключа:
   - ключ отключает необходимость ручного одобрения изменений перед применением.
   - это может привести к незапланированным изменениям, особенно в промышленной среде
   
   Может быть полезным:
   - при разработке и тестировании
   - при автоматизации  

   Вывод команды `docker ps`
   
![4](https://github.com/user-attachments/assets/e1d119e3-2373-45dc-b13c-77037cae198c)


7. Уничтожение ресурсов с помощью terraform.
   `terraform destroy`
   
   Cодержимое файла terraform.tfstate:

```HCL
{
  "version": 4,
  "terraform_version": "1.11.3",
  "serial": 11,
  "lineage": "1c6f547c-38db-984c-c7c5-e6201214fd22",
  "outputs": {},
  "resources": [],
  "check_results": null
}
```
8. Docker-образ nginx:latest не был удален, так как в файле main.tf для этого ресурса было установлено `keep_locally = true`.
 
 А из [инструкции](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image) следует, что такие образы не удаляются.
 
   ![5](https://github.com/user-attachments/assets/d3c33ecf-d815-4ab1-a227-b95f77ede313)

