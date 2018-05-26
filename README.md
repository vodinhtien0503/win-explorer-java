Tên: Võ Đình Tiến
Mã SV:17IT105
ĐỀ SỐ:06




VIDEO hướng dẫn sử dụng :			


Hướng dẫn chạy code..  
1.TẢI PROJECT thiguaki TRÊN GITHUB


2.MỞ CMD



3.DÙNG LỆNH CD CHUYỂN ĐẾN THƯ MỤC CHƯA SOURCE CODE



VÍ DỤ:
cd C:\Users\Administrator\eclipse-workspace\thiguaki\src



4.BIÊN DỊCH BẰNG DÒNG LỆNH JAVAC


javac thiguaki\winexplorer.java



5.CHẠY LỆNH BẰNG LỆNH JAVA


java thiguaki.winexplorer




DefaultMutableTreeNode root = new DefaultMutableTreeNode();//tạo thể hiện có tên root đóng vai trò là gốc của cây//



//khởi tạo cây và thêm cây vào jscrollpane
 treeModel = new DefaultTreeModel(root);              
 tree = new JTree(treeModel);                           
 tree = new JTree(treeModel);
 tree.setRootVisible(false);
 tree.addTreeSelectionListener(treeSelectionListener);
 tree.setCellRenderer(new FileTreeCellRenderer());
 tree.expandRow
 JScrollPane treeScroll = new JScrollPane(tree);  //




//đăng ký bộ lắng nghe xử lí sự kiện phát hiện nút được chọn hay không được chọn và lấy dữ liêu liên quan đến các nút cha nút con
 TreeSelectionListener treeSelectionListener = new TreeSelectionListener() {
                public void valueChanged(TreeSelectionEvent tse){
                    DefaultMutableTreeNode node =
                        (DefaultMutableTreeNode)tse.getPath().getLastPathComponent();
                    showChildren(node);
                    setFileDetails((File)node.getUserObject());
                }
        

    };//

 File[] roots = fileSystemView.getRoots();//trả về phân vùng gốc của hệ thống//
//


            for (File fileSystemRoot : roots) {
                DefaultMutableTreeNode node = new DefaultMutableTreeNode(fileSystemRoot);
                root.add( node );//tệp tin nằm trong phân vùng của hệ thống thì khi đó tạo nút mới và thêm vào gốc của cây
           


   
                File[] files = fileSystemView.getFiles(fileSystemRoot, true);//lấy danh sách file được thể hiện trong hệ thống
                for (File file : files) {
                    if (file.isDirectory()) {
                        node.add(new DefaultMutableTreeNode(file));//danh sách file nếu có thư mục thì tạo ra nút và thêm vào gốc cây




///
private TreePath findTreePath(File find) {
        for (int ii=0; ii<tree.getRowCount(); ii++) {
            TreePath treePath = tree.getPathForRow(ii);
            Object object = treePath.getLastPathComponent();//lấy đối tượng đại diện cho nút được chọn và trả vè bởi tree.getSelectionpath()
            DefaultMutableTreeNode node = (DefaultMutableTreeNode)object;
            File nodeFile = (File)node.getUserObject();

            if (nodeFile==find) {
                return treePath;
            }
        }
       
        return null;
    }thiết lập đường dãn cho nút được chọn//



//
private void deleteFile() {
        if (currentFile==null) {
            showErrorMessage("khong co tep nao de xoa.","chon tep");//khi tệp được chọn rỗng thì hiện thông báo
            return;
        }

        int result = JOptionPane.showConfirmDialog(
            gd,
            "ban co muon xoa tep?",
            "xoa tep",
            JOptionPane.ERROR_MESSAGE
            );
        if (result==JOptionPane.OK_OPTION) {//khi ngừoi dùng xác nhận muốn xóa
            try {
                System.out.println("currentFile: " + currentFile);hiển thị tệp ra màn hình
                TreePath parentPath = findTreePath(currentFile.getParentFile());//đường dẫn trong cây
                System.out.println("parentPath: " + parentPath);
                DefaultMutableTreeNode parentNode =
                    (DefaultMutableTreeNode)parentPath.getLastPathComponent();
                System.out.println("parentNode: " + parentNode);//đường dẫn nút trỏ đến//

             


 ///  
                boolean directory = currentFile.isDirectory();
                boolean deleted = currentFile.delete();
                if (deleted) {
                    if (directory) {
                   
                        TreePath currentPath = findTreePath(currentFile);
                        System.out.println(currentPath);
                        DefaultMutableTreeNode currentNode =
                            (DefaultMutableTreeNode)currentPath.getLastPathComponent();

                        treeModel.removeNodeFromParent(currentNode);
                    } nếu xóa thư mục thì hiển thị đường dẫn trong cây và đường dẫn nút được trỏ đến///





                    showChildren(parentNode);
               //
                    } else {
                    String msg = "tep '" +
                        currentFile +
                        "' khong the xoa tep.";
                    showErrorMessage(msg,"xoa that bai");ngược lại hiển thị thông báo lỗi //
                }
            } catch(Throwable t) {
                showThrowable(t);
            }
        }
        gd.repaint();
    }

    ///tạo mối quan hệ nút con với gốc cây
    private void showChildren(final DefaultMutableTreeNode node) {
        tree.setEnabled(false);
        progressBar.setVisible(true);
        progressBar.setIndeterminate(true);

        SwingWorker<Void, File> worker = new SwingWorker<Void, File>() {
            @Override
            public Void doInBackground() {
                File file = (File) node.getUserObject();
                if (file.isDirectory()) {
                    File[] files = fileSystemView.getFiles(file, true); 
                    if (node.isLeaf()) {
                        for (File child : files) {
                            if (child.isDirectory()) {
                                publish(child);
                            }
                        }
                    }
                    setTableData(files);
                }
                return null;
            }
            ////

            
        // tạo bộ xuât ô các icon lá khác nhau    
            class FileTreeCellRenderer extends DefaultTreeCellRenderer {

                private FileSystemView fileSystemView;

                private JLabel label;

                FileTreeCellRenderer() {
                    label = new JLabel();
                    label.setOpaque(true);
                    fileSystemView = FileSystemView.getFileSystemView();
                }

                @Override
                public Component getTreeCellRendererComponent(
                    JTree tree,
                    Object value,
                    boolean selected,
                    boolean expanded,
                    boolean leaf,
                    int row,
                    boolean hasFocus) {

                    DefaultMutableTreeNode node = (DefaultMutableTreeNode)value;
                    File file = (File)node.getUserObject();
                    label.setIcon(fileSystemView.getSystemIcon(file));
                    label.setText(fileSystemView.getSystemDisplayName(file));
                    label.setToolTipText(file.getPath());

                    if (selected) {
                        label.setBackground(backgroundSelectionColor);
                        label.setForeground(textSelectionColor);
                    } else {
                        label.setBackground(backgroundNonSelectionColor);
                        label.setForeground(textNonSelectionColor);
                    }

                    return label;
                }
            }


        /// đẩy dữ liêu vào các hàng các cột    
            public Object getValueAt(int row, int column) {
                File file = files[row];
                switch (column) {
                    case 0:
                        return fileSystemView.getSystemIcon(file);
                    case 1:
                        return fileSystemView.getSystemDisplayName(file);
                    case 2:
                        return file.getPath();
                    case 3:
                        return file.length()+"  bytes  ";
                    case 4:
                        return file.lastModified();
                        
             ///thêm chức năng           
                        
                    case 5:
                        return file.canRead();// tệp có thể đọc 
                    case 6:
                        return file.canWrite();//tệp có thể viết
                    case 7:
                        return file.canExecute();// tệp có thể truy cập
                    case 8:
                        return file.isDirectory();//tệp là thư mục
                    case 9:
                        return file.isFile();//là tệp tin
                     ///   sau đó check vào checkbox các cột có tên 
                        "R",
                        "W",
                        "E",
                        "D",
                        "F",
                    default:
                        System.err.println("loi");
                }
                return "";
            }///
            
            
          //   int row = table.getSelectionModel().getLeadSelectionIndex();
                    setFileDetails( ((FileTableModel)table.getModel()).getFile(row) );
           Lấy hàng được chon và lấy giá trị tại vị trí click trong bảng ///
            
            
            







         
