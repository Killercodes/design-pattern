**Blackboard Design Pattern**
===

The Blackboard design pattern is a design pattern used in software engineering to coordinate separate, disparate systems that need to work together or in sequence, continually cycling for updates and actions. The blackboard consists of a number of stores or “global variables”. This is similar to a repository of messages, which can be accessed by separate autonomous processes. A “Controller” monitors the properties on the blackboard and can decide which “Knowledge Sources” to prioritize1.

A common example of this pattern is in speech recognition. Separate threads can process different parts of the sound sample, updating the blackboard with words that have been recognized. Then another process can pick up these words and perform grammar and sentence formation2.

Here’s an example of the Blackboard design pattern implemented in C# for a Radar Defense System.

## Example 1
```cs
public class Blackboard
{
    public string Data { get; set; }
    public event EventHandler DataChanged;
    protected virtual void OnDataChanged()
    {
        if (DataChanged != null) DataChanged(this, EventArgs.Empty);
    }
}

public class KnowledgeSource
{
    public Blackboard Blackboard { get; set; }
    public void Blackboard_DataChanged(object sender, EventArgs e)
    {
        // Do something with the data
        // Update the blackboard with new data
        Blackboard.Data = "new data";
    }
}

public class Controller
{
    public Blackboard Blackboard { get; set; }
    public List<KnowledgeSource> KnowledgeSources { get; set; }
    public void Blackboard_DataChanged(object sender, EventArgs e)
    {
        // Decide which knowledge source to use based on the data
        KnowledgeSource selectedKnowledgeSource = KnowledgeSources[0];
        selectedKnowledgeSource.Blackboard_DataChanged(sender, e);
    }
}


```

## Example 2
```cs
public class BlackBoard
    {
        public List<KnowledgeWorker> knowledgeWorkers;
        protected Dictionary<string, ControlData> data;
        public Control control;


        public BlackBoard()
        {
            this.knowledgeWorkers = new List<KnowledgeWorker>();
            this.control = new Control(this);
            this.data = new Dictionary<string, ControlData>();
        }

        public void addKnowledgeWorker(KnowledgeWorker newKnowledgeWorker) 
        {
            newKnowledgeWorker.blackboard = this;
            this.knowledgeWorkers.Add(newKnowledgeWorker);
        }       

        public Dictionary<string, ControlData> inspect()
        {
            return (Dictionary<string, ControlData>) this.data.ToDictionary(k => k.Key, k => (ControlData) k.Value.Clone());
        }
        public void update(KeyValuePair<string, ControlData> blackboardEntry) 
        {
            if (this.data.ContainsKey(blackboardEntry.Key))
            {
                this.data[blackboardEntry.Key] = blackboardEntry.Value;
            }
            else
                throw new InvalidOperationException(blackboardEntry.Key + " Not Found!");
        }

        public void update(string key, ControlData data)
        {
            if (this.data.ContainsKey(key))
            {
                this.data[key] = data;
            }
            else
            {
                this.data.Add(key, data);
            }            
        }

        public void print()
        {
            System.Console.WriteLine("Blackboard state");
            foreach (KeyValuePair<string, ControlData> cdata in this.data)
            {
                Console.WriteLine(string.Format("data:{0}", cdata.Key));
                Console.WriteLine(string.Format("\tProblem:{0}", cdata.Value.problem));
                if(cdata.Value.input!=null)
                    Console.WriteLine(string.Format("\tInput:{0}", string.Join(",",cdata.Value.input)));
                if(cdata.Value.output!=null)
                    Console.WriteLine(string.Format("\tOutput:{0}", string.Join(",",cdata.Value.output)));
            }
        }

    }
```
