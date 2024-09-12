- 👋 Hi, I’m @iibarraflores
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
iibarraflores/iibarraflores is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import pandas as pd

pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)

# Read the CSV file into a DataFrame, starting from the second row and setting the header
df = pd.read_csv('Tabulador de Compensaciones y Beneficios - Equipo plan Operadores.xlsx - HC  Ok.csv', header=1)

# Remove trailing whitespaces from `P. Vac. nueva ` column
df['P. Vac. nueva '] = df['P. Vac. nueva '].astype(str).str.strip()

# Convert the column `P. Vac. nueva ` to numeric
df['P. Vac. nueva '] = pd.to_numeric(df['P. Vac. nueva '],errors = 'coerce')

# Display the first 5 rows
print(df.head().to_markdown(index=False, numalign="left", stralign="left"))

# Print the column names and their data types
print(df.info())

# Function to generate a personalized letter
def generate_letter(row):
    # Extract relevant data from the row (using the corrected column name)
    nombre = row['Nombre']
    nueva_posicion = row['Nueva posición']
    ingresos_netos_semanales = row['Ingresos netos sem. Actuales']
    ingresos_diarios_netos = row['Ingresos Diarios Netos Actuales']
    ingresos_netos_mensuales_28 = row['IMSD  28 DÍAS']
    ingresos_netos_mensuales_30 = row['IMSD 30 DÍAS']
    ingresos_netos_mensuales_31 = row['IMSD 31 DÍAS']
    prima_vacacional_nueva = row['P. Vac. nueva ']
    aguinaldo_nuevo = row['AG. Nuevo']
    descanso_cumpleanos = row['Descanso por Cumple - Días']
    licencia_paternidad = row['Licencia Paternidad']

    # Format the letter
    letter = f"""
Estimado/a {nombre},

¡Felicitaciones por tu nuevo puesto como {nueva_posicion}! Tu dedicación y talento han sido fundamentales para nuestro equipo, y estamos emocionados de verte asumir este nuevo rol clave en el marco de nuestro proyecto de crecimiento.

**Tus nuevos beneficios incluyen:**

**Beneficios Monetarios:**

| Concepto | Cantidad |
|---|---|
| Ingresos netos semanales | ${ingresos_netos_semanales} |
| Ingresos netos diarios | ${ingresos_diarios_netos} |
| Ingresos netos mensuales (28 días) | ${ingresos_netos_mensuales_28} |
| Ingresos netos mensuales (30 días) | ${ingresos_netos_mensuales_30} |
| Ingresos netos mensuales (31 días) | ${ingresos_netos_mensuales_31} |
| Prima vacacional | {prima_vacacional_nueva} |
| Días de aguinaldo | {aguinaldo_nuevo} |

**Beneficios no Monetarios:**

* Disfrutarás de {descanso_cumpleanos} día(s) libre(s) por tu cumpleaños para celebrarlo como prefieras.
* En caso de ser padre, contarás con una licencia de paternidad de {licencia_paternidad} días para disfrutar de esos momentos especiales.

Gracias por tu compromiso continuo. ¡Estamos ansiosos por construir juntos un futuro exitoso!

Atentamente,

[Nombre del remitente]
[Cargo del remitente]
"""
    return letter

# Iterate and generate letters
for index, row in df.iterrows():
    letter = generate_letter(row)
    print(letter)
    print("---") # Separator between letters
