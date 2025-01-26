# 09-kuber
Для настройки RBAC нужно создать 2 манифеста Role/RoleBinding

Role:
Описывает к каким ресурсам можно получить доступ и что с ними можно делать.
Манифест:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: default
  name: pod-logs-viewer
rules:
  - apiGroups: [""]
    resources:
      - pods
      - pods/log
    verbs:
      - get
      - list
      - watch
```

RoleBinding:
Описываем пользователя и связываем его с ролью:
Манифест:
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: default
  name: pod-logs-viewer-binding
subjects:
  - kind: User
    name: logger
    apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-logs-viewer
  apiGroup: rbac.authorization.k8s.io
```

Применяем и видим что пользователь создан и связан с ролью:
![image](https://github.com/user-attachments/assets/4d2c68ab-b53b-4194-9854-32fcfd3722c9)

Стартанем тестовый deploymetn из предыдущих примеров, что бы добавить Pod's в NS
![image](https://github.com/user-attachments/assets/13c514d9-db17-4c1e-90af-e9a8d375f2bc)

Проверим что бы можем подключаться с помощью этого пользователя:
![image](https://github.com/user-attachments/assets/55b9b304-8d9c-435e-b265-2eedba21fb36)

Как видим в NS: default мы можем подключиться , в kube-system нет

Вытащим поды которые есть в NS default и сможем ли получить список из kube-system:
![image](https://github.com/user-attachments/assets/2b19a679-f943-47d3-abf6-6605d01726f3)

Получим логи пода:
![image](https://github.com/user-attachments/assets/840f98af-1dea-42cd-9c13-e71a9dd3727f)

Получим данные пода:
![image](https://github.com/user-attachments/assets/ed5f7592-a0c9-465f-aa72-5d9577cb5793)


Что права отрабатывают правильно, попробуем удалить pod:
![image](https://github.com/user-attachments/assets/fe556556-6141-4710-8d1e-5b92a5e419fb)

Получаем ошибку, связано это с установленным нами манифестом Role.
