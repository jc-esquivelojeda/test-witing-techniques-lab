#Laboratorio 2 Aplicacion de tecnicas efectivas de escritura de pruebas

## Escenario 1
```
TEST UserAuthentication
  ASSERT_TRUE(authenticate("validUser", "validPass"), "Should succeed with correct credentials")
  ASSERT_FALSE(authenticate("validUser", "wrongPass"), "Should fail with wrong credentials")
END TEST
```
## Análisis:

El anterior escenario plantea validar la  autenticacion de usuarios,  se identificaron las siguientes mejoras:
- El nombre de la prueba debe ser mas descriptivo ya que no refleja el escenario de prueba especifico a realizar
- Se identificaron 2 casos de prueba distintos dentro del mismo metodo por lo que se aconseja separarlos

## Corrección:
En base a los pasos para escribir buenas pruebas se aplico lo siguiente:

- 1- Definir objetivos claros, 2- nombres claros y 3- aislamiento de casos de prueba: Se separaron los escenarios en 2 casos distintos debido a que se probaban 2 criterios diferentes. y  se marco un nombre descriptivo sobre los casos a validar 
- 4- Se implemento la logica en pseudocodigo para inicializar las pruebas 
- 5- Se agregaron pruebas para  los casos extremos al validar algunas combinaciones extra como: el usuario es incorrecto y otro donde ambas credenciales no son validas, o bien que se envian las credenciales vacias

 A continuación se muestra el pseudocodigo con la corrección:
```
SETUP
  INITIALIZE_USERS()
END_SETUP

TEARDOWN
  TEARDOWN_USERS()
END_TEARDOWN

TEST test_userAuthentication_when_valid_user_and_password
  ASSERT_TRUE(authenticate("validUser", "validPass"), "Should succeed with correct credentials")  
END TEST

TEST test_userAuthentication_when_valid_user_and_invalid_password
  ASSERT_FALSE(authenticate("validUser", "wrongPass"), "Should fail with wrong password")
END TEST

TEST test_userAuthentication_when_invalid_user_and_valid_password
  ASSERT_FALSE(authenticate("wrongUser", "validPass"), "Should fail with wrong user")
END TEST

TEST test_userAuthentication_when_invalid_user_and_invalid_password
  ASSERT_FALSE(authenticate("wrongUser", "wrongPass"), "Should fail with wrong user and password")
END TEST

TEST test_userAuthentication_when_empty_credentials
  ASSERT_FALSE(authenticate("", ""), "Should fail with empty credentials")
END TEST
```


## Escenario 2

```
TEST DataProcessing
  DATA data = fetchData()
  TRY
    processData(data)
    ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
  CATCH error
    ASSERT_EQUALS("Data processing error", error.message, "Should handle processing errors")
  END TRY
END TEST
```

## Análisis:
En el anterior pseudocodigo se desea validar el procesamiento de informacion recibida
- El nombre de la prueba debe ser mas descriptivo ya que no refleja el escenario de prueba especifico a realizar: validar el procesamiento de los datos (aplica 1 y 2 objetivos claros y nombrado descriptivo)
- La informacion base a cargar debe ser inicializada de ser posible en la preparacion de las pruebas o inyectada como una dependencia (aplica 4 implementacion de setup y teardown )
- Se identificaron 2 casos de prueba distintos dentro del mismo metodo por lo que se aconseja separar la logica excepcional de la del flujo para el happy path ( aplica 3 - aislamiento de casos)
- Se agregaron casos adicionales de prueba (aplica 5- manejo de excepciones y casos extremos)


## Corrección:
```
// se cargan los datos al inicializar las pruebas
SETUP
  DATA data = fetchData()
END_SETUP

// se liberan los datos
TEARDOWN
  clear(data);
END_TEARDOWN
  
TEST test_process_correct_data  
  processData(data)
  ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
END TEST

TEST test_process_invalid_data
  TRY
    processData(data)
  CATCH error
    ASSERT_ERROR("Data processing error", error.message, "Should fail with invalid data")
  END TRY
END TEST

TEST process_null_data
  TRY
    processData(null)
  CATCH error
    ASSERT_ERROR("Data processing error", error.message, "Should fail with null data")
  END TRY
END TEST
```


## Escenario 3

```
TEST UIResponsiveness
  UI_COMPONENT uiComponent = setupUIComponent(1024)
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(1024), "UI should adjust to width of 1024 pixels")
END TEST
```

## Análisis:

- El nombre de la prueba debe ser mas descriptivo ya que no refleja el escenario de prueba especifico a realizar: se desea validar que la app es responsiva (1 y 2 objetivos claros y nombrado descriptivo)
- La informacion base a cargar debe ser inicializada de ser posible en la preparacion de las pruebas o inyectada como una dependencia (3 implementacion de setup y teardown )
- Se identificaron 2 casos de prueba distintos dentro del mismo metodo por lo que se aconseja separar la logica excepcional de la del flujo para el happy path (3 - aislamiento de casos)
- Se agregaron casos adicionales de prueba (5- manejo de excepciones y casos extremos)


## Corrección:
```

SETUP
  UI_COMPONENT uiComponent = setupUIComponent()
END_SETUP

TEARDOWN
  clearUIComponent()
END_TEARDOWN

TEST test_UIResponsiveness_with_1024_pixels
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(1024), "UI should adjust to width of 1024 pixels")
END TEST

TEST test_UIResponsiveness_with_2048_pixels
  ASSERT_FALSE(uiComponent.adjustsToScreenSize(2048), "UI should fail to width of 2048 pixels")
END TEST

TEST test_UIResponsiveness_with_null_value
  TRY
    uiComponent.adjustsToScreenSize(null)
  CATCH error
    ASSERT_ERROR("UI REsponsiveness error", error.message, "Should fail with null data")
  END TRY
END TEST

```


