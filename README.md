Command
public interface ICommand
{
    void Execute();
    void Undo();
}
public class Luz
{
    public void Encender() => Console.WriteLine("💡 Luz encendida");
    public void Apagar() => Console.WriteLine("💡 Luz apagada");
}
public class EncenderLuzCommand : ICommand
{
    private Luz _luz;
    
    public EncenderLuzCommand(Luz luz) => _luz = luz;

    public void Execute() => _luz.Encender();
    
    public void Undo() => _luz.Apagar();
}

public class ApagarLuzCommand : ICommand
{
    private Luz _luz;

    public ApagarLuzCommand(Luz luz) => _luz = luz;

    public void Execute() => _luz.Apagar();

    public void Undo() => _luz.Encender();
}
public class ControlRemoto
{
    private ICommand _comando;

    public void SetCommand(ICommand comando) => _comando = comando;

    public void PresionarBoton() => _comando.Execute();

    public void PresionarDeshacer() => _comando.Undo();
}
class Program
{
    static void Main()
    {
        // Crear el receptor
        Luz luz = new Luz();
        
        // Crear los comandos
        ICommand encender = new EncenderLuzCommand(luz);
        ICommand apagar = new ApagarLuzCommand(luz);
        
        // Crear el invocador
        ControlRemoto control = new ControlRemoto();

        // Asignar y ejecutar comandos
        control.SetCommand(encender);
        control.PresionarBoton();  // 💡 Luz encendida

        control.SetCommand(apagar);
        control.PresionarBoton();  // 💡 Luz apagada

        // Deshacer última acción
        control.PresionarDeshacer();  // 💡 Luz encendida
    }
}

Iterator
public interface IIterator<T>
{
    bool HasNext();  // Verifica si hay más elementos
    T Next();        // Devuelve el siguiente elemento
}
public interface IAggregate<T>
{
    IIterator<T> CreateIterator();  // Devuelve un iterador para la colección
}
public class ConcreteAggregate<T> : IAggregate<T>
{
    private List<T> _items = new List<T>();

    // Método para agregar elementos
    public void Add(T item)
    {
        _items.Add(item);
    }

    // Implementación de CreateIterator
    public IIterator<T> CreateIterator()
    {
        return new ConcreteIterator<T>(_items);
    }
}
public class ConcreteIterator<T> : IIterator<T>
{
    private List<T> _items;
    private int _position = 0;

    public ConcreteIterator(List<T> items)
    {
        _items = items;
    }

    public bool HasNext()
    {
        return _position < _items.Count;
    }

    public T Next()
    {
        if (HasNext())
        {
            return _items[_position++];
        }
        else
        {
            throw new InvalidOperationException("No more elements.");
        }
    }
}
public class Program
{
    public static void Main(string[] args)
    {
        // Crear una colección
        var aggregate = new ConcreteAggregate<int>();
        aggregate.Add(1);
        aggregate.Add(2);
        aggregate.Add(3);
        aggregate.Add(4);
        
        // Obtener el iterador
        var iterator = aggregate.CreateIterator();
        
        // Recorrer la colección
        while (iterator.HasNext())
        {
            Console.WriteLine(iterator.Next());
        }
    }
}
1
2
3
4

