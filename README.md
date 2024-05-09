# Ejercicios-PDD-demo-Sigleton-Decorator-en-Csharp-o-Python
# Armando Montiel Garcia #20210602
# Descripción de la Actividad
Objetivo: Aplicar los patrones de diseño Singleton y Decorador para construir un sistema de configuración de productos personalizados en una tienda en línea.
# CODIGO:
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using static System.Net.Mime.MediaTypeNames;

namespace ConsoleApp1
{
    // Interfaz Producto
    interface IProducto
    {
        string GetDescripcion();
        double GetPrecio();
    }

    // Clase ConfiguracionProducto (Singleton)
    class ConfiguracionProducto
    {
        private static ConfiguracionProducto instancia;
        private ConfiguracionProducto() { }

        public static ConfiguracionProducto ObtenerInstancia()
        {
            if (instancia == null)
            {
                instancia = new ConfiguracionProducto();
            }
            return instancia;
        }

        // Otros métodos para manejar la configuración del producto
    }

    // Clase ProductoBase
    class ProductoBase : IProducto
    {
        private string descripcion;
        private double precio;

        public ProductoBase(string descripcion, double precio)
        {
            this.descripcion = descripcion;
            this.precio = precio;
        }

        public string GetDescripcion()
        {
            return descripcion;
        }

        public double GetPrecio()
        {
            return precio;
        }
    }

    // Clase Decorador abstracta
    abstract class Decorador : IProducto
    {
        protected IProducto producto;

        public Decorador(IProducto producto)
        {
            this.producto = producto;
        }

        public virtual string GetDescripcion()
        {
            return producto.GetDescripcion();
        }

        public virtual double GetPrecio()
        {
            return producto.GetPrecio();
        }
    }

    // Clase ColorDecorador
    class ColorDecorador : Decorador
    {
        private string color;
        private double precioExtra;

        public ColorDecorador(IProducto producto, string color, double precioExtra) : base(producto)
        {
            this.color = color;
            this.precioExtra = precioExtra;
        }

        public override string GetDescripcion()
        {
            return base.GetDescripcion() + ", Color: " + color;
        }

        public override double GetPrecio()
        {
            return base.GetPrecio() + precioExtra;
        }
    }

    // Clase AccesoriosDecorador
    class AccesoriosDecorador : Decorador
    {
        private List<string> accesorios;
        private double precioExtra;

        public AccesoriosDecorador(IProducto producto, List<string> accesorios, double precioExtra) : base(producto)
        {
            this.accesorios = accesorios;
            this.precioExtra = precioExtra;
        }

        public override string GetDescripcion()
        {
            string descripcion = base.GetDescripcion() + ", Accesorios:";
            foreach (string accesorio in accesorios)
            {
                descripcion += " " + accesorio;
            }
            return descripcion;
        }

        public override double GetPrecio()
        {
            return base.GetPrecio() + (accesorios.Count * precioExtra);
        }
    }

    // Ejemplo de uso
    class Program
    {
        static void Main(string[] args)
        {
            // Obtener la instancia única de ConfiguracionProducto
            ConfiguracionProducto configuracion = ConfiguracionProducto.ObtenerInstancia();

            // Crear un producto base
            IProducto producto = new ProductoBase("Smartphone", 500);

            // Decorar el producto con opciones de color y accesorios
            producto = new ColorDecorador(producto, "Negro", 10);
            List<string> accesorios = new List<string> { "Estuche", "Cargador" };
            producto = new AccesoriosDecorador(producto, accesorios, 20);

            // Mostrar la descripción y el precio final del producto
            Console.WriteLine("Descripción: " + producto.GetDescripcion());
            Console.WriteLine("Precio: $" + producto.GetPrecio());
            Console.ReadKey();
        }
        
    }

}

![image](https://github.com/ArmandoMontielGarcia1/Ejercicios-PDD-demo-Sigleton-Decorator-en-Csharp-o-Python/assets/144396511/77e466af-534b-42a9-8970-eee6bc8425d8)

