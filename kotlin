class BTree<Key : Comparable<Key>, Value> {
    private val t = 2   // Minimum degree of the B-Tree node
    private var root: Node<Key, Value>? = null

    private class Node<Key : Comparable<Key>, Value>(var n: Int = 0, var leaf: Boolean = false) {
        var keys = arrayOfNulls<Key>(2 * t - 1)
        var values = arrayOfNulls<Value>(2 * t - 1)
        var children = arrayOfNulls<Node<Key, Value>>(2 * t)

        // Insert key-value pair in this node
        // Split the node if it's full
        fun insertNonFull(key: Key, value: Value) {
            var i = n - 1
            if (leaf) {
                while (i >= 0 && keys[i]!! > key) {
                    keys[i + 1] = keys[i]
                    values[i + 1] = values[i]
                    i--
                }
                keys[i + 1] = key
                values[i + 1] = value
                n++
            } else {
                while (i >= 0 && keys[i]!! > key) {
                    i--
                }
                i++
                if (children[i]!!.n == 2 * t - 1) {
                    splitChild(i)
                    if (keys[i]!! < key) {
                        i++
                    }
                }
                children[i]!!.insertNonFull(key, value)
            }
        }

        // Split the ith child of this node
        private fun splitChild(i: Int) {
            val newNode = Node<Key, Value>(t - 1, leaf)
            val child = children[i]
            children[i] = newNode

            for (j in 0 until t - 1) {
                newNode.keys[j] = child!!.keys[j + t]
                newNode.values[j] = child.values[j + t]
                newNode.children[j] = child.children[j + t]
            }
            newNode.children[t - 1] = child!!.children[2 * t - 1]
            newNode.n = t - 1
            child.n = t - 1
            for (j in n downTo i + 1) {
                keys[j] = keys[j - 1]
                children[j + 1] = children[j]
            }
            keys[i] = child.keys[t - 1]
            children[i + 1] = newNode
            n++
        }
    }

    // Insert key-value pair in the B-Tree
    fun insert(key: Key, value: Value) {
        val r = root
        if (r!!.n == 2 * t - 1) {
            val newNode = Node<Key, Value>(t = 1, leaf = false)
            root = newNode
            newNode.children[0] = r
            newNode.splitChild(0)
            newNode.insertNonFull(key, value)
        } else {
            r.insertNonFull(key, value)
        }
    }
