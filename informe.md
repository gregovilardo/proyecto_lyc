# Reglas de Inferencia
 
## Introducción 

Las *reglas de inferencia* (inference rules) se utilizan para especificar que *juicios de tipo* (type judgements) son validos. Antes de empezar, asumiendo que el lector acaba de terminar con la materia Lenguajes y Compiladores de la FaMAF, intentaremos acortar la brecha de desconocimiento que posee, o mejor dicho, la brecha de conocimiento que le falta, introduciendo:  

\frac{\pi \vdash e_0 : \theta \to \theta' \quad \pi \vdash e_1 : \theta}{\pi \vdash e_0 \, e_1 : \theta'}

- El *tipo primitivo* **int** 
