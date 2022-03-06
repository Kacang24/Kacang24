    private val clickListener: ReaderClickListener
) : RecyclerView.Adapter<ReaderHolder>() {

    private var readers: List<Reader> = listOf()

    fun updateReaders(readers: List<Reader>) {
        this.readers = readers
        notifyDataSetChanged()
    }

    override fun getItemCount(): Int {
        return readers.size
    }

    override fun onBindViewHolder(holder: ReaderHolder, position: Int) {
        holder.view.text = readers[position].serialNumber
        holder.view.setOnClickListener {
            clickListener.onClick(readers[position])
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ReaderHolder {
        val view = LayoutInflater.from(parent.context)
            .inflate(R.layout.list_item_reader, parent, false) as MaterialButton
        return ReaderHolder(view)
    }
}

class ReaderClickListener(val activityRef: WeakReference<MainActivity>) {
    fun onClick(reader: Reader) {
        val connectionConfig =
            ConnectionConfiguration.InternetConnectionConfiguration(true)

        val readerCallback = object: ReaderCallback {
            override fun onSuccess(reader: Reader) {
                activityRef.get()?.let {
                    it.runOnUiThread {
                        // Update UI with connection success
                        it.updateReaderConnection(isConnected = true)
                    }
                }
            }

            override fun onFailure(e: TerminalException) {
                activityRef.get()?.let {
                    it.runOnUiThread {
                        // Update UI with connection failure
                        Toast.makeText(
                            it,
                            "Failed to connect to reader",
                            Toast.LENGTH_SHORT)
                            .show()
                    }
                }
            }
        }
        Terminal.getInstance().connectInternetReader(
            reader,
            connectionConfig,
            readerCallback,
        )
    }
}
