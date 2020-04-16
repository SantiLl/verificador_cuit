# verificador_cuit
Procedimiento para obtener el dígito verificador

Para correr el codigo `ruby verificar_cuit.rb`

```ruby
def validar_cuit(cuit) # Usuario ingresa el cuit compuesto por XY-12345678-Z
  cuit = cuit.to_s # Transformo el input en string
  cuit.delete!('-') # Eliminar los guiones, puntos, y, espacios.
  cuit.delete!('.')
  cuit.delete!(' ')

  if cuit.length != 11 # Chequeo que si o si tenga 11 caracteres
    puts "El cuit no cuenta con 11 cifras (tiene #{cuit.length})."
    return false
  end

  unless cuit.scan(/\D/).empty? # Chequeo si tiene algun simbolo no numerico
    puts "El cuit cuenta con caracteres no deseados (#{cuit}). Solo puede tener numeros."
    return false
  end

  base = [5, 4, 3, 2, 7, 6, 5, 4, 3, 2] # Serie de numeros por el cual se hace el calculo
  cuit_array = cuit.split('') # Convierto el string en array.
  last_cuit_number = cuit_array.delete_at(-1)# Elimino Z del cuit
  aux = 0 # creo la variable la cual va a tener el resultado

  cuit_array.zip(base).each do |c_num, b_num| # junto ambos arrays.
    aux += c_num.to_f * b_num # Multiplico la iteracion y lo almaceno en la variable
    puts "Multiplicacion de #{c_num} y #{b_num}"
  end

  divided_aux = aux / 11 # Divido la suma total por 11 (el codigo verificador)
  result = aux.round - (divided_aux.round * 11)
  codigo_verificador = 11 - result # Resultado de codigo verificador
  first_two = cuit_array[0] + cuit_array[1] # Reuno XY

  puts "Result #{result}, codigo_verificador #{codigo_verificador}, last_cuit_number, #{last_cuit_number}"

  if result > 1 && codigo_verificador == last_cuit_number.to_i # Chequeo si el resultado es valido
    puts "El codigo verificador (#{codigo_verificador}) es correcto! "
    return true
  elsif result <= 1 # Caso en el que hay un error de la formula, el codigo verificador no puede ser 10
    case codigo_verificador
    when 9
      if first_two.to_i == 23 || first_two.to_i == 33
        puts "El codigo verificador (9) es correcto" # Digito verificador pasa a ser 9 para hombres/empresas
        return true
      end
    when 4
      if first_two.to_i == 23
        puts "El codigo verificador (4) es correcto" # Digito verificador pasa a ser 4 para mujeres
        return true
      end
    when 3
      if first_two.to_i == 23 || first_two.to_i == 33
        puts "El codigo verificador (3) es correcto" # Digito verificador pasa a ser 3 para empresas/humanos repetidos
        return true
      end
    else
      puts "El cuit ingresado es incorrecto o hay un error en la verificacion, revisar si el ingresado (#{cuit}) es el correcto."
      return false
    end
  else
    puts "El cuit ingresado es incorrecto o hay un error en la verificacion, revisar si el ingresado (#{cuit}) es el correcto."
    return false
  end
end
```
