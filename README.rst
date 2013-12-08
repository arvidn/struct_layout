struct_layout
=============

tool to show the structure layout of types in your C/C++ program,
highlighting padding.

It recreates the structure layout based on DWARF debug information from a
binary or object file. This tool analyzes the output from ``dwarfdump`` which
will need to be installed for ``struct_layout`` to work.

On Mac, debug symbols are not typically linked into the final executable,
but this tool can still be used on individual object and .dSYM files.

It is typically a good idea to pipe the output from ``struct_layout`` to less.
Since the default is to colorize the output::

	./struct_layout.py <object-file>.o | less -r

usage
-----

::

	usage: ./struct_layout.py [options] exe-file [name-prefix-filter]
	
	exe-file must have DWARF debug symbols in it. It
	may be an object file, shared library or executable. On Mac
	dsymutils will be invoked for files with no direct debug symbols
	in them.
	
	name-prefix-filter is an optional argument. When
	specified, only types whose prefix matches this are
	printed. Names are fully qualified and start with ::
	to denote the global scope.
	
	OPTIONS
	-a           print all types, including standard library
	             and implementation detail types
	-c           disable color output
	-p <file>    use the specified access_profile output file
	             to display use counts for only instrumented types
	
	the dwarfdump tool is a dependency and need to be
	installed on your system. On Mac OS X you may need dsymutil
	in order to link debug symbols together

Output is colorized by default, use ``-c`` to disable.
Types belonging to the standard library and compiler specific libraries
are not printed by default. To see those types as well, pass in ``-a``.

The optional filter simply filters the prefix of types. This is primarily
useful if you're only interested in types from a specific namespace. For
instance, passing in ``::boost::`` will only print types defined in the
``boost`` namespace.

The ``-p`` option takes the profile output generated from access_profiler_.

.. _access_profiler: https://github.com/arvidn/access_profiler

example output
--------------

.. parsed-literal::

	struct **::libtorrent::i2p_connection** [104 Bytes]
	     : [shared_ptr<libtorrent::i2p_stream> : 16] m_sam_socket
	    0:   [i2p_stream* : 8] px
	     :   [shared_count : 8] pn
	    8:     [sp_counted_base* : 8] pi\_
	     : [proxy_settings : 40] m_sam_router
	     :   [basic_string<char> : 8] hostname
	     :     [_Alloc_hider : 8] _M_dataplus
	     :       [allocator<char> : 1] <base-class>
	   16:         [new_allocator<char> : 1] <base-class>
	   16:       [char* : 8] _M_p
	   24:   [int : 4] port
	   **--- 4 Bytes padding ---**
	     :   [basic_string<char> : 8] username
	     :     [_Alloc_hider : 8] _M_dataplus
	     :       [allocator<char> : 1] <base-class>
	   32:         [new_allocator<char> : 1] <base-class>
	   32:       [char* : 8] _M_p
	     :   [basic_string<char> : 8] password
	     :     [_Alloc_hider : 8] _M_dataplus
	     :       [allocator<char> : 1] <base-class>
	   40:         [new_allocator<char> : 1] <base-class>
	   40:       [char* : 8] _M_p
	   48:   [enum proxy_type : 4] type
	   52:   [bool : 1] proxy_hostnames
	   53:   [bool : 1] proxy_peer_connections
	   **--- 2 Bytes padding ---**
	     : [basic_string<char> : 8] m_i2p_local_endpoint
	     :   [_Alloc_hider : 8] _M_dataplus
	     :     [allocator<char> : 1] <base-class>
	   56:       [new_allocator<char> : 1] <base-class>
	   56:     [char* : 8] _M_p
	     : [basic_string<char> : 8] m_session_id
	     :   [_Alloc_hider : 8] _M_dataplus
	     :     [allocator<char> : 1] <base-class>
	   64:       [new_allocator<char> : 1] <base-class>
	   64:     [char* : 8] _M_p
	     : ["list<std::pair<std::basic_string<char>, boost::function<void (const boost::system::error_code &, const char * : 16] m_name_lookup
	     :   ["_List_base<std::pair<std::basic_string<char>, boost::function<void (const boost::system::error_code &, const char * : 16] <base-class>
	     :     [_List_impl : 16] _M_impl
	     :       ["allocator<std::_List_node<std::pair<std::basic_string<char>, boost::function<void (const boost::system::error_code &, const char * : 1] <base-class>
	   72:         ["new_allocator<std::_List_node<std::pair<std::basic_string<char>, boost::function<void (const boost::system::error_code &, const char * : 1] <base-class>
	     :       [_List_node_base : 16] _M_node
	   72:         [_List_node_base* : 8] _M_next
	   80:         [_List_node_base* : 8] _M_prev
	   88: [enum state_t : 4] m_state
	   **--- 4 Bytes padding ---**
	   96: [io_service& : 8] m_io_service


