

# PostgreSQL数据库连接参数

//你的账号密码
    DB_HOST = 'localhost'
    DB_PORT = '5432'
    DB_NAME = 'rzdb'
    DB_USER = 'postgres'
    DB_PASS = '123456'   

# 模式名称

//因为是pgsql，不写这个进public里了。
SCHEMA_NAME = 'supplyeleservicedb'   


# 检测数据库连接

//
def create_connection():
    try:
        conn = psycopg2.connect(host=DB_HOST, port=DB_PORT, database=DB_NAME, user=DB_USER, password=DB_PASS)
        print("数据库连接成功！")
        return conn
    except OperationalError as e:
        print(f"数据库连接失败: {e}")
        return None


# 创建表，如果不存在

//创建表，填写表信息
def create_table(cursor):
    create_table_query = f"""
    CREATE TABLE IF NOT EXISTS {SCHEMA_NAME}.rzyh_photo (
        id SERIAL PRIMARY KEY,
        name VARCHAR(255),
        img TEXT
    );
    """
    cursor.execute(create_table_query)
    print("表已成功创建或已存在。")


# 图片文件夹路径

//你图片的路径
PHOTO_DIR = r"C:\Users\DaShan\Desktop\PHOTO\PHOTO\照片"     



# 遍历文件夹中的所有文件

//支持的图片格式
for filename in os.listdir(PHOTO_DIR):
    if filename.lower().endswith(('.png', '.jpg', '.jpeg', '.gif', '.bmp')):  
        file_path = os.path.join(PHOTO_DIR, filename)

        // 将图片转换为Base64字符串
        with open(file_path, 'rb') as image_file:
            encoded_string = base64.b64encode(image_file.read()).decode('utf-8')

        // 将名称和Base64字符串插入数据库
        cursor.execute(f"INSERT INTO {SCHEMA_NAME}.rzyh_photo (name, img) VALUES (%s, %s)", (filename, encoded_string))

