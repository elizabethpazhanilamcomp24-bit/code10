# code10
bst
class CityNode:
    def __init__(self, name, population):
        self.name = name
        self.population = population
        self.left = None
        self.right = None


class CityBST:
    def __init__(self):
        self.root = None

    # Insert city
    def insert(self, name, population):
        self.root = self._insert(self.root, name, population)

    def _insert(self, node, name, population):
        if node is None:
            return CityNode(name, population)
        if name < node.name:
            node.left = self._insert(node.left, name, population)
        elif name > node.name:
            node.right = self._insert(node.right, name, population)
        else:
            print(f"City '{name}' already exists. Use update_population to change the population.")
        return node

    # Delete city
    def delete(self, name):
        self.root = self._delete(self.root, name)

    def _delete(self, node, name):
        if node is None:
            print(f"City '{name}' not found.")
            return None
        if name < node.name:
            node.left = self._delete(node.left, name)
        elif name > node.name:
            node.right = self._delete(node.right, name)
        else:
            # Node to delete found
            if node.left is None:
                return node.right
            elif node.right is None:
                return node.left
            # Node with two children
            min_larger_node = self._min_value_node(node.right)
            node.name, node.population = min_larger_node.name, min_larger_node.population
            node.right = self._delete(node.right, min_larger_node.name)
        return node

    def _min_value_node(self, node):
        current = node
        while current.left:
            current = current.left
        return current

    # Update population
    def update_population(self, name, new_population):
        node = self._search(self.root, name)
        if node:
            node.population = new_population
            print(f"Updated population of {name} to {new_population}")
        else:
            print(f"City '{name}' not found.")

    # Search city (returns node)
    def _search(self, node, name):
        if node is None or node.name == name:
            return node
        if name < node.name:
            return self._search(node.left, name)
        else:
            return self._search(node.right, name)

    # Display cities in ascending or descending order
    def display(self, order="asc"):
        print(f"\nCities in {'ascending' if order == 'asc' else 'descending'} order:")
        self._display(self.root, order)

    def _display(self, node, order):
        if node:
            if order == "asc":
                self._display(node.left, order)
                print(f"{node.name}: {node.population}")
                self._display(node.right, order)
            elif order == "desc":
                self._display(node.right, order)
                print(f"{node.name}: {node.population}")
                self._display(node.left, order)

    # Maximum comparisons to find a city
    def max_comparisons(self):
        height = self._get_height(self.root)
        print(f"Maximum comparisons needed to find a city: {height}")

    def _get_height(self, node):
        if node is None:
            return 0
        left_height = self._get_height(node.left)
        right_height = self._get_height(node.right)
        return 1 + max(left_height, right_height)


# Example usage:
if __name__ == "__main__":
    bst = CityBST()

    # Adding cities
    bst.insert("New York", 8419600)
    bst.insert("Los Angeles", 3980400)
    bst.insert("Chicago", 2716000)
    bst.insert("Houston", 2328000)
    bst.insert("Phoenix", 1690000)

    # Display
    bst.display(order="asc")
    bst.display(order="desc")

    # Update population
    bst.update_population("Chicago", 2800000)

    # Delete a city
    bst.delete("Phoenix")

    # Display after update and delete
    bst.display(order="asc")

    # Maximum comparisons
    bst.max_comparisons()
