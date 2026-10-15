Эксперименты с qwen code (qwen 3.7 plus), spec kit.
Используя обвязку qwen code поверх моделей qwen и sdd-tool spec kit тренируемся в настройке полного цикла разработки ПО. 
Некоторые решения будут оставаться в отдельных ветках для удобной возможности смотреть дифф

Установка qwen code


Установка spec kit (https://github.github.com/spec-kit/installation.html)
В системе должен стоять Python 3.11+
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z  (в моем случае @vX.Y.Z -> v1.2.0)
specify init . --integration qwen  (выполняется из консоли в папке проекта (repo root)) - это создаст папку .specify в корне
<img width="861" height="214" alt="image" src="https://github.com/user-attachments/assets/21b13159-d385-44c2-8a14-ccfb59861a3c" />

