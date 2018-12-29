#include<string>
#include<iostream>
#include<fstream>
#include<algorithm>

typedef unsigned int Type;

struct vertex;

struct edge
{
	Type distance;
	vertex *destination;
	edge *next;
};

struct vertex
{
	std::string name;
	edge *edgeHead;
	vertex *Next;
};

struct Index
{
	vertex *vertices;
	int index;
	int weight;
	Index *next;
	//Index *prev;
};
struct priorityqueue
{
	Index* FrontIndex;
	priorityqueue *Next;
};

bool Check(vertex *& Head, std::string NameOfCity)
{
	auto p = Head;
	while (p->Next)
	{
		if (p->name == NameOfCity)
			return 0;
		p = p->Next;
	}
	return 1;
}

void AddVertex(vertex *& Head, std::string NameOfCity)
{
	auto p = Head;
	vertex *New = new vertex{ NameOfCity, nullptr, nullptr };
	while (p->Next)
		p = p->Next;
	p->Next = New;
	New->Next = nullptr;
}

void AddEdge(vertex *& Head, edge *& EdgeHead, std::string Start, std::string Destination, Type Distance)
{
	auto p = Head;
	auto ptr = Head;

	while (p->name != Start && p->Next)
		p = p->Next;
	p->edgeHead = new edge{ Distance, nullptr, p->edgeHead };
	while (ptr->Next && ptr->name != Destination)
		ptr = ptr->Next;
	p->edgeHead->destination = ptr;
}	

void DataInput(vertex *& Head, edge *& Edge, std::string FileName)
{
	std::string Start, Destination;
	Type Distance;
	std::ifstream RoutesFile(FileName);
	RoutesFile >> Start >> Destination >> Distance;
	edge *New = new edge{ Distance, nullptr, nullptr };
	if (!Head)
		Head = new vertex{ Start, nullptr, nullptr };
	while (RoutesFile >> Start >> Destination >> Distance)
	{
		if (Check(Head, Start))
			AddVertex(Head, Start);
	}
	RoutesFile.clear();
	RoutesFile.seekg(std::ios_base::beg);
	while (RoutesFile >> Start >> Destination >> Distance)
	{
		if (Check(Head, Destination))
			AddVertex(Head, Destination);
	}
	RoutesFile.clear();
	RoutesFile.seekg(std::ios_base::beg);
	while (RoutesFile >> Start >> Destination >> Distance)
	{
		//if(Checkv2(Head, Start, Destination))
		AddEdge(Head, Edge, Start, Destination, Distance);
	}
}

void Output(vertex * Head){
	if (Head)
	{
		std::cout << Head->name << "\n";
		while (Head->edgeHead)
		{
			std::cout << " " << Head->edgeHead->destination->name << "  " << Head->edgeHead->distance;
			Head->edgeHead = Head->edgeHead->next;
		}
		std::cout << "\n";
		Output(Head->Next);
	}
}

void AddIndex(vertex *&Head, vertex *ptrStart, Index *&First, int n) // zle dziala za malo elem
{
	auto p = Head;
	auto ptrFirst = First;
	for (int num = 1; num < n; num++)
	{
		Index *New = new Index{ p, num, INT_MAX, nullptr };
			if (p == ptrStart)
				num--;
			else 
			{
				while (ptrFirst->next)
					ptrFirst = ptrFirst->next;
				ptrFirst->next = New;
				New->next = nullptr;
			}
		p = p->Next;
	}
}

void AddToPriorityQueue(Index *pointer, Index *First, Index *neighbour, priorityqueue *&beginning)
{
	priorityqueue *tmp = new priorityqueue{ First, nullptr };
	auto p = beginning;
	auto temp = tmp;
	temp->FrontIndex = neighbour;
	if (!beginning || temp->FrontIndex->weight < beginning->FrontIndex->weight)
	{
		tmp->FrontIndex = neighbour;
		tmp->Next = beginning;
		beginning = tmp;
	}
	else
	{
		auto ptr = beginning;
		while (ptr->Next && ptr->Next->FrontIndex->weight <= temp->FrontIndex->weight)
			ptr = ptr->Next;
		tmp->Next = ptr->Next;
		ptr->Next = tmp;
	}
}

void AddWithPriority(Index *First, priorityqueue *& beginning)
{
	auto temp = First,
		 tempFirst = First;
	auto p = First->vertices->edgeHead;
	int n = 0;
	while (tempFirst->vertices->edgeHead)
	{
		tempFirst->vertices->edgeHead = tempFirst->vertices->edgeHead->next;
		n++;
	}
	tempFirst->vertices->edgeHead = p;
	Index **New = new Index *[n];
	int j = 0;
	while (tempFirst->vertices->edgeHead)
	{
		while (temp->next)
		{
			if (First->vertices->edgeHead->destination != temp->vertices->Next)
				temp = temp->next;
			else
			{	
				New[j]= temp->next;
				j++;
				break;
			}
		}
		tempFirst->vertices->edgeHead = tempFirst->vertices->edgeHead->next;
		temp = First;
	}
	std::cout << std::endl;
	tempFirst->vertices->edgeHead = p;
	First->vertices->edgeHead = p;
	int num = 0;
	while(First->vertices->edgeHead)
	{
		New[num]->weight = First->vertices->edgeHead->distance;
		First->vertices->edgeHead = First->vertices->edgeHead->next;
		num++;
	}
	First->vertices->edgeHead = p;

	//-------------------------------------
	for(int licz = 0; licz < num; licz++)
		std::cout << New[licz]->vertices->name << "  " << New[licz]->weight << "\n";
	//-------------------------------------
	
	priorityqueue *tmp = new priorityqueue{ First, nullptr };

	auto pointer = First;
	for (int i = 0; i < j; i++)
	{
		AddToPriorityQueue(pointer, First, New[i], beginning);
		pointer = pointer->next;
	}
	
}




void Dijkstra(vertex *& Head, std::string RoutesToFind)
{
	auto p = Head;
	int n = 0; // number of vertices in graph
	while (p->Next)
	{
			n++;
		p = p->Next;
	}
	p = Head;
	std::cout << n << "\n";
	std::string Start, Destination;
	std::ifstream RoutesToSearch(RoutesToFind);
	int numerator = 0, u = 0, j = 0;

	RoutesToSearch >> Start >> Destination;
	while (p->Next && Start != p->name)
		p = p->Next;
	Index *First = new Index{ p, 0, 0, nullptr };
	auto ptrFirst = First,
		 tmp = First;
	auto ptr = p;
	int *Q = new int[n];
	int *S = new int[n];
	
	priorityqueue *Beginning = nullptr;
	p = Head;
	int num = 1;

	AddIndex(Head, ptr, First, n);
	for (int i = 0; i < n; i++)
	{	
		Q[i] = ptrFirst->index;
		ptrFirst = ptrFirst->next;
		std::cout << Q[i] << "\n";
	}
	int *prev = new int[n];

	AddWithPriority(First, Beginning);
}

int main(int argv, char **argc)
{
	vertex *Head = nullptr;
	edge *Edge = nullptr;
	DataInput(Head, Edge, "Text1.txt");
	Dijkstra(Head, "Mapa.txt");
	Output(Head);

	system("pause");
}
