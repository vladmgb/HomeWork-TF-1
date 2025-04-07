# Домашнее задание к занятию «Введение в Terraform»

### Чек-лист готовности к домашнему заданию
Cкриншот вывода команды ```terraform --version```
![1](https://github.com/user-attachments/assets/22804ce8-620a-4e52-9770-908391586dc3)

Убедитесь, что в вашей ОС установлен docker.
![2](https://github.com/user-attachments/assets/33f920c8-a83a-4574-bb85-273219903390)

### Задание 1

2.Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,токены итд)
personal.auto.tfvars

3. Cекретное содержимое созданного ресурса random_password:
`"result": "iPgGDgMyrgZUch9V"`

4.
`terraform validate`

4.1 Error: Missing name for resource

   on main.tf line 29, in resource "docker_image":
   29: resource "docker_image" {

 All resource blocks must have 2 labels (type, name).


4.2 Error: Invalid resource name

   on main.tf line 34, in resource "docker_container" "1nginx":
   34: resource "docker_container" "1nginx" {

 A name must start with a letter or underscore and may contain only letters, digits, underscores,   
 and dashes.

 4.3 Error: Reference to undeclared resource

   on main.tf line 36, in resource "docker_container" "nginx":
   36:   name  = "example_${random_password.random_string_FAKE.resulT}"

 A managed resource "random_password" "random_string_FAKE" has not been declared in the root  
 module.


Исправленный фрагмент коды:

```HCL
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"
```

5.
   Вывод команды `docker ps`

![3](https://github.com/user-attachments/assets/8c874eec-47c2-47cf-8fdb-52d3a52e4d78)


6. Объясните своими словами, в чём может быть опасность применения ключа -auto-approve. Догадайтесь или нагуглите зачем может пригодиться данный ключ?
   Вывод команды `docker ps`

![4](https://github.com/user-attachments/assets/1c88d9f0-bcfe-43fe-81b5-4f6d028a9e2d

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




