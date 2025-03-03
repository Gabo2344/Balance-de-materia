# Balance-de-materia
Balance con reacción química - calculo
def obtener_datos_reaccion():
    print("Ingrese la ecuación química balanceada (ejemplo: 2 H2 + 1 O2 -> 2 H2O):")
    ecuacion = input().strip()
    
    num_reactivos = int(input("Ingrese el número de reactivos: "))
    reactivos = {}

    for _ in range(num_reactivos):
        nombre = input("Nombre del reactivo: ").strip()
        cantidad = float(input(f"Cantidad inicial de {nombre} (moles): "))
        coef = float(input(f"Coeficiente estequiométrico de {nombre} en la ecuación: "))
        reactivos[nombre] = {"cantidad": cantidad, "coef": coef}

    conversion = float(input("Ingrese la conversión del reactivo limitante en porcentaje: ")) / 100

    return ecuacion, reactivos, conversion


def identificar_limitante(reactivos):
    relaciones = {nombre: datos["cantidad"] / datos["coef"] for nombre, datos in reactivos.items()}
    reactivo_limitante = min(relaciones, key=relaciones.get)
    return reactivo_limitante


def calcular_productos_y_restantes(reactivos, reactivo_limitante, conversion):
    moles_react_limitante = reactivos[reactivo_limitante]["cantidad"]
    coef_limitante = reactivos[reactivo_limitante]["coef"]
    moles_reaccionados = moles_react_limitante * conversion

    factores_esteq = {nombre: datos["coef"] / coef_limitante for nombre, datos in reactivos.items()}

    for nombre in reactivos:
        reactivos[nombre]["cantidad_restante"] = reactivos[nombre]["cantidad"] - (factores_esteq[nombre] * moles_reaccionados)

    productos = {
        "H2O": 2 * moles_reaccionados  # Solo un ejemplo con agua como producto
    }

    return productos, reactivos


def main():
    ecuacion, reactivos, conversion = obtener_datos_reaccion()
    reactivo_limitante = identificar_limitante(reactivos)
    productos, reactivos = calcular_productos_y_restantes(reactivos, reactivo_limitante, conversion)

    print("\nResultados de la Reacción:")
    print(f"Reactivo limitante: {reactivo_limitante}")
    
    print("\nProductos formados:")
    for producto, cantidad in productos.items():
        print(f"{producto}: {cantidad:.2f} moles")

    print("\nReactivos restantes:")
    for nombre, datos in reactivos.items():
        print(f"{nombre}: {datos['cantidad_restante']:.2f} moles")


if __name__ == "__main__":
    main()
