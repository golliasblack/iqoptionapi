#Para opciones binarias


## comprar

comprar una opción binaria

### buy()

Ejemplo

```python
from iqoptionapi.stable_api import IQ_Option
import logging
import time
logging.basicConfig(level=logging.DEBUG,format='%(asctime)s %(message)s')
I_want_money=IQ_Option("email","pass")
goal="EURUSD"
print("get candles")
print(I_want_money.get_candles(goal,60,111,time.time()))
Money=1
ACTIVES="EURUSD"
ACTION="call"#or "put"
expirations_mode=1

check,id=I_want_money.buy(Money,ACTIVES,ACTION,expirations_mode)
if check:
    print("!buy!")
else:
    print("buy fail")
```

```python
I_want_money.buy(Money,ACTIVES,ACTION,expirations)
                #Money:Cantidad de dinero type(int)
                #ACTIVES:Ejemplo de entrada "EURUSD" OR "EURGBP".... puedes ver todos loa ACTIVE con -> get_all_ACTIVES_OPCODE
                #ACTION:"call"/"put" type(str) call -> sube, put -> baja
                #expirations:Introduce minutos, cuidado con los timpos muy largos ya que fallará al comprar (Tiempo de cierre de merrcado) Gracias a Darth-Carrotpie's code (int)https://github.com/Lu-Yi-Hsun/iqoptionapi/issues/6
                #return:if sucess return (True,id_number) esle return(Fale,None) 
```
### buy_multi()

```python
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#conectar a iqoption
Money=[]
ACTIVES=[]
ACTION=[]
expirations_mode=[]

Money.append(1)
ACTIVES.append("EURUSD")
ACTION.append("call")
expirations_mode.append(1)

Money.append(1)
ACTIVES.append("EURAUD")
ACTION.append("call")
expirations_mode.append(1)

print("buy multi")
id_list=I_want_money.buy_multi(Money,ACTIVES,ACTION,expirations_mode)

print("check win only one id (id_list[0])")
print(I_want_money.check_win_v2(id_list[0],2))
```
### buy_by_raw_expirations()

buy the binary optoin by expired

```python
price=2
active="EURUSD"
direction="call"
option="turbo"#binary
expired=1293923# Este tiempo de expiración necesitas contarlo o obtenerlo por tu mismo
I_want_money.buy_by_raw_expirations(price, active, direction, option,expired)
```

## get_remaning()

purchase time=remaning time - 30
```python
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#conectar a iqoption
Money=1
ACTIVES="EURUSD"
ACTION="call"#or "put"
expirations_mode=1
while True:
    remaning_time=I_want_money.get_remaning(expirations_mode)
    purchase_time=remaning_time-30
    if purchase_time<4:#comprar las opciones binarias con un tiempo de compra menor a 4 min
        I_want_money.buy(Money,ACTIVES,ACTION,expirations_mode)
        break
```

## sell_option()

```python
I_want_money.sell_option(sell_all)#Introduce int o una lista de id de órdenes
```
Ejemplo

```python
from iqoptionapi.stable_api import IQ_Option
import time
print("login...")
I_want_money=IQ_Option("email","password")
I_want_money.connect()#conectar a iqoption
Money=1
ACTIVES="EURUSD"
ACTION="call"#or "put"
expirations_mode=1

id=I_want_money.buy(Money,ACTIVES,ACTION,expirations_mode)
id2=I_want_money.buy(Money,ACTIVES,ACTION,expirations_mode)

time.sleep(5)
sell_all=[]
sell_all.append(id)
sell_all.append(id2)
print(I_want_money.sell_option(sell_all))
```

## check win

Entrará en bucle hasta que la respuesta sea ganadora (win) o sin beneficios (loose)

### check_win()

```python
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#connect to iqoption
check,id = I_want_money.buy(1, "EURUSD", "call", 1)
print("Empezando a comprobar la operación...")
print(I_want_money.check_win(id))
``` 

```python
I_want_money.check_win(23243221)
#""Necesitas obtener el id_number de la función buy()""
#I_want_money.check_win(id_number)
#Esta función entrará en un bucle hasta que el resultado sea: win/equal/loose
```

### check_win_v2()

```python
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#conectar a iqoption
check,id = I_want_money.buy(1, "EURUSD", "call", 1)
print("Empezando a comprobar la operación...")
polling_time=3
print(I_want_money.check_win_v2(id,polling_time))
```

### check_win_v3()

Mejor camino para comprobar el resultado de
la operación

```python
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#connect to iqoption
check,id = I_want_money.buy(1, "EURUSD", "call", 1)
print("Empezando a comprobar la operación...")
print(I_want_money.check_win_v3(id))
```
 
## get_binary_option_detail()
![](expiration_time.png)

sample
```python
from iqoptionapi.stable_api import IQ_Option
print("login...")
I_want_money=IQ_Option("email","password")
I_want_money.connect()#connect to iqoption
d=I_want_money.get_binary_option_detail()
print(d["CADCHF"]["turbo"])
print(d["CADCHF"]["binary"])
```
## get_all_init()

get_binary_option_detail es la base en eta api

Tu obtendrás los detalle sobre la opción binaria

```
I_want_money.get_all_init()
```

## get_all_profit()

sample

```python
from iqoptionapi.stable_api import IQ_Option
print("login...")
I_want_money=IQ_Option("email","password")
I_want_money.connect()#connect to iqoption
d=I_want_money.get_all_profit()
print(d["CADCHF"]["turbo"])
print(d["CADCHF"]["binary"])
```

Si tu quieres saber el profit en tiempo real
[get real time profit](/all/#get_commission_change)

## get_betinfo()

Si la opción no cierra todavía o el id es incorrecto, devolverá False
if order not close yet or wrong id it will return False
```python
isSuccessful,dict=I_want_money.get_betinfo(4452272449)
#I_want_money.get_betinfo 
#ENTRADA: order id
#SALIDA:isSuccessful,dict
```
## get_optioninfo

### get_optioninfo()

Introduce cuántos datos quieres obtener del historial de Trading (solo para opciones binarias)

```python
print(I_want_money.get_optioninfo(10))
```

### get_optioninfo_v2()

Introduce cuántos datos quieres obtener del historial de Trading (solo para opciones binarias)

```python
print(I_want_money.get_optioninfo_v2(10))
```
### get_option_open_by_other_pc()

Obtener si tu cuenta tiene otra sesión abierta (puede ser en otro pc) y está abriendo opciones

Tu puedes obtener la opción con esta función

```python
import time
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#conectar a iqoption
while True:
    #Por favor accede a la web de iqoption y abre alguna opción binaria
    if I_want_money.get_option_open_by_other_pc()!={}:
        break
    time.sleep(1)
print("Obtener la opción abierta de otra sesión y de la misma cuenta")
print(I_want_money.get_option_open_by_other_pc())

id=list(I_want_money.get_option_open_by_other_pc().keys())[0]
I_want_money.del_option_open_by_other_pc(id)
print("Depués de la id")
print(I_want_money.get_option_open_by_other_pc())
```

## Obtener indicador

### Ejemplo¡

```python
from iqoptionapi.stable_api import IQ_Option
I_want_money=IQ_Option("email","password")
I_want_money.connect()#connect to iqoption
goal="EURUSD"
I_want_money.start_mood_stream(goal)
print(I_want_money.get_traders_mood(goal))
I_want_money.stop_mood_stream(goal)
```

### start_mood_stream()

```python
I_want_money.start_mood_stream(goal)
```

### get_traders_mood()

call get_traders_mood() after start_mood_stream

```python
I_want_money.get_traders_mood(goal)
```

### get_all_traders_mood()

Obtendrá todo el indicador de operaciones en que hayas activado el stream

```python
I_want_money.get_all_traders_mood()
#output:(dict) all mood you start
```

### stop_mood_stream()

Si no está usando el indicador, porfavor páralo para una mejor conexión.

```python
I_want_money.stop_mood_stream(goal)
```

